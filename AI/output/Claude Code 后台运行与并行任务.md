## 核心总结

在 Claude Code 中，**后台运行 Subagent / 任务**是实现多任务并行、长时间自动化、上下文隔离的关键能力。根据场景选择合适方式：临时耗时任务用 Ctrl+B；并行上下文探索用 `/fork`；周期性任务用 `/loop`（会话内）或 Routines（持久云端）；大规模并行改动用 `/batch` + Worktree。

---

## 1. 主要后台运行方式

| 方式                      | 适用场景                | 操作方式                                            | 特点与限制                                                  |
| ----------------------- | ------------------- | ----------------------------------------------- | ------------------------------------------------------ |
| **`/fork`**             | 基于现有上下文的并行探索        | `/fork <指令>`                                    | 继承完整上下文 + prompt cache，自动后台运行；不能再向下嵌套 fork；需 v2.1.117+ |
| **Ctrl+B**              | 将正在运行的任务快速转后台       | 任务执行中按 Ctrl+B                                   | 立即释放当前窗口；tmux 用户需按两次                                   |
| **`/loop`**             | 会话内周期性重复任务          | `/loop 5m <指令>`                                 | 最小间隔 1 分钟；7 天自动过期；带 jitter 防峰值                         |
| **`background: true`**  | 自定义 Subagent 默认后台运行 | 在 `.claude/agents/` YAML frontmatter 中设置        | 结合 `isolation: worktree` 效果最佳                          |
| **`--worktree` / `-w`** | 独立 Git 工作树隔离        | `claude --worktree <name>` 或 `claude -w <name>` | 文件与分支完全隔离，默认从 `origin/HEAD` 创建分支                       |
| **Routines**            | 云端持久化定时任务           | `/schedule <描述>`                                | 最小间隔 1 小时；关机后仍在云端继续运行；支持 Schedule / API / GitHub 三种触发器 |
| **`/batch`**            | 大规模并行代码改动           | `/batch <指令>`                                   | 自动分解为 5–30 个独立单元，每单元在独立 Worktree 中并行执行，每个单元独立开 PR      |

---

## 2. 监控与管理后台任务

- **`claude agents`**（终端命令）：打开 **Agent View**，一屏查看所有后台 session 的状态（Needs Input / Working / Completed）、消息数、Git 分支、项目路径。
- **`/tasks`**（会话内命令）：列出当前会话内的后台任务。
- **`/workflows`**：查看动态 Workflow 的实时进度树。
- **`/statusline`**：配置终端状态栏，可显示的字段包括：

  | 字段 | 内容 |
  |------|------|
  | `model` | 当前模型名 |
  | `contexta`–`contexte` | 上下文用量（ASCII 条、数值、百分比、状态词等多种格式） |
  | `costfmt` | 本次会话费用 |
  | `duration` | 会话时长 |
  | `branch` / `hash` / `status` | Git 状态信息 |

- **日志/Transcript 位置**：`~/.claude/projects/<project>/<session-id>.jsonl`，JSONL 格式，记录所有消息、工具调用、权限请求。Subagent transcript 单独存放于 `~/.claude/projects/<project>/<session-id>/subagents/agent-<id>.jsonl`。默认保留 30 天（可通过 `cleanupPeriodDays` 配置）。
- **通知**：任务完成或需要权限确认时触发终端/桌面提醒。

---

## 3. 高级配置：自定义 Subagent（`.claude/agents/`）

```yaml
---
name: my-worker
description: 执行独立子任务，Claude 应主动委派
background: true          # 始终后台运行
isolation: worktree       # 在独立 Git 目录中运行（强烈推荐并行写文件时使用）
tools: [Bash, Read]       # 限制可用工具
model: sonnet             # 可指定模型
permissionMode: auto      # 权限模式
maxTurns: 50
---

# Subagent 系统 Prompt
在此描述 subagent 的行为规则...
```

**完整 frontmatter 字段速查**（常用）：

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | String | 唯一标识符（必填） |
| `description` | String | 委派触发描述（必填） |
| `background` | Boolean | `true` = 始终后台运行 |
| `isolation` | String | `worktree` = 独立 Git 工作树 |
| `tools` | List | 限制可用工具列表 |
| `disallowedTools` | List | 明确禁用的工具 |
| `model` | String | `sonnet` / `opus` / `haiku` / 完整 model ID |
| `permissionMode` | String | `default` / `auto` / `acceptEdits` / `plan` 等 |
| `maxTurns` | Integer | 最大 agentic 轮次 |
| `memory` | String | 跨会话记忆范围：`user` / `project` / `local` |
| `effort` | String | `low` / `medium` / `high` / `max` |
| `color` | String | Agent View 中的显示颜色 |

> **注意**：`context: fork` 不是有效字段。继承上下文通过 `/fork` 命令实现，而非 frontmatter 配置。

---

## 4. `/loop` 详解

```
/loop                      # 动态间隔，内置维护 prompt
/loop 5m                   # 固定 5 分钟，内置 prompt
/loop 5m 检查部署是否完成   # 固定 5 分钟，自定义指令
/loop 检查 CI 状态          # 动态间隔，Claude 自行决定频率
```

- **Jitter**：周期任务在计划时间后最多延迟 30 分钟触发（每小时以内的任务延迟为间隔的一半）；单次任务若在整点附近则最多提前 90 秒。
- **过期**：周期任务 **7 天**后自动过期（触发最后一次后删除）。
- **自定义 prompt**：在 `~/.claude/loop.md` 或 `.claude/loop.md` 中定义默认循环内容。
- **停止**：等待下次触发期间按 `Esc` 取消。
- **持久化**：`/loop` 仅在会话内有效；跨机器/关机后持续运行需使用 **Routines**（`/schedule`）。

---

## 5. Routines（云端定时任务）

Routines 运行在 Anthropic 托管的云端基础设施上，**与本地 session 无关**，关机后仍继续执行。

**三种触发器**：

| 触发器 | 说明 | 示例 |
|--------|------|------|
| Schedule | 定时（最小间隔 1 小时） | 每天 9am 生成 PR Review |
| API | 外部 HTTP POST 触发 | CI 流水线通知 |
| GitHub | 仓库事件触发 | PR 合并后自动同步 |

**创建方式（CLI）**：

```
/schedule daily PR review at 9am
/schedule list
/schedule run <routine-name>
/schedule update <routine-name>
```

- 运行计入订阅用量，每账号有每日上限（查看：`claude.ai/code/routines`）。
- 网络访问：默认 Trusted 模式，允许常见开发域名；自定义域名需在环境设置中添加。

---

## 6. Git Safety Protocol（内置安全规则）

**受保护路径**（默认需要权限确认或拒绝写入）：

- `.git`、`.gitconfig`、`.gitmodules`
- Shell 配置：`.bashrc`、`.zshrc` 等
- 包管理器：`.npmrc`、`.yarnrc` 等
- IDE 配置：`.vscode`、`.idea`、`.devcontainer` 等
- `.claude/`（`.claude/worktrees/` 除外）

**Auto 模式 Classifier** 自动拦截：

- `curl | bash` 类代码下载执行
- 将敏感数据发送到外部端点
- 生产部署与数据库迁移
- 强制推送或推送到 `main`
- 批量删除云存储数据
- IAM / 权限修改

**权限模式速查**：

| 模式 | 行为 |
|------|------|
| `default` | 每个操作均提示确认 |
| `acceptEdits` | 文件编辑 + 常见 fs 命令自动批准 |
| `plan` | 只读，不执行任何写操作 |
| `auto` | Classifier 后台评估，低风险自动放行 |
| `dontAsk` | 仅预授权工具，其余自动拒绝 |
| `bypassPermissions` | 跳过所有检查（仅限隔离容器/VM） |

Auto 模式：连续被拦截 3 次或累计 20 次后自动降级为手动确认提示。

---

## 7. 最佳实践

- **临时重活**：直接运行 → Ctrl+B → 继续主线程。
- **并行逻辑探索**：`/fork` + `background: true`（共享 prompt cache，成本更低）。
- **会话内周期检查**：`/loop`（7 天内有效）。
- **跨会话 / 关机后持续**：Routines（`/schedule`，最小 1 小时间隔）。
- **大规模并行改动**：`/batch`（自动分解 5–30 单元，各自独立 Worktree + PR）。
- **只读研究**：`Explore` 子代理 + 后台，避免主上下文膨胀。
- **安全与隔离**：写文件时始终使用 `isolation: worktree`；权限模式使用 `auto` 或 `acceptEdits`；敏感路径预配置保护规则。
