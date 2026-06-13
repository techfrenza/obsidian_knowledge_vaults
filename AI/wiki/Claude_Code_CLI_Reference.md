---
title: Claude Code CLI Reference
aliases: ["Claude Code CLI", "斜杠命令速查", "CLI 命令参考", "Claude Code 命令行"]
tags: [claude-code, cli, commands, reference, slash-commands]
category: claude-tooling
parent: "[[Claude_Code_Commands]]"
created: 2026-06-07
date: "2026-06-07"
---

# Claude Code CLI Reference

Parent: [[Claude_Code_Commands]]

> 核心论点：Claude Code CLI 提供三层命令体系：启动命令（管理会话生命周期）、斜杠命令（会话内工作流控制）、CLI flags（行为定制）。掌握完整命令集是从 Level 2 用户迈向 Level 4+ 的关键。

[Source: raw/CLI 命令与斜杠命令速查.md]

---

## 一、CLI 启动命令（Terminal 层）

| 命令 | 说明 | 示例 |
|------|------|------|
| `claude` | 启动交互式会话 | `claude` |
| `claude "query"` | 带初始 prompt 启动 | `claude "explain this project"` |
| `claude -p "query"` | 非交互模式（SDK 输出后退出） | `claude -p "explain function"` |
| `cat file \| claude -p "query"` | 管道输入 | `cat logs.txt \| claude -p "explain"` |
| `claude -c` | 继续最近会话 | `claude -c` |
| `claude -r "name" "query"` | 按名称恢复会话 | `claude -r "auth-refactor" "Finish PR"` |
| `claude update` | 更新到最新版本 | `claude update` |
| `claude -n "name"` | 命名当前会话（可后续 resume） | `claude -n "feature-login"` |
| `claude agents` | 列出所有配置的 subagents | `claude agents` |
| `claude mcp` | 配置 MCP 服务器 | - |
| `claude plugin` | 管理插件（别名：claude plugins） | `claude plugin install code-review@...` |
| `claude remote-control` | 远程控制服务器模式 | `claude remote-control --name "My Project"` |
| `claude auth login` | 登录账号（`--console` 使用 API Key） | `claude auth login` |

---

## 二、CLI 关键 Flags

| Flag | 说明 |
|------|------|
| `--permission-mode plan` | 以计划模式启动（不执行，仅规划）|
| `--permission-mode auto` | 自动模式（需 Team/Enterprise，Sonnet 4.6+）|
| `--dangerously-skip-permissions` | 跳过所有权限提示（仅沙盒环境）|
| `--model sonnet` / `--model opus` | 指定模型别名 |
| `--effort high` / `xhigh` / `max` | 设置推理深度（session 内有效）|
| `--max-turns N` | 限制 agent 轮数（print 模式）|
| `--max-budget-usd 5.00` | 限制 API 花费上限 |
| `--bare` | 最小模式：跳过 hooks/skills/MCP/CLAUDE.md 自动发现，脚本调用加速 |
| `--worktree feature-name` | 在隔离 git worktree 中启动 |
| `--add-dir ../apps ../lib` | 添加额外工作目录 |
| `--system-prompt "text"` | 替换整个系统提示 |
| `--append-system-prompt "text"` | 追加到默认系统提示（保留内置能力）|
| `--json-schema '{...}'` | 强制 JSON Schema 结构化输出 |
| `--agent-teams` | 启用实验性 Agent Teams |
| `--agents '{"name":{...}}'` | 动态定义 subagents（JSON 格式）|
| `--fork-session` | resume 时创建新 session ID，不覆盖原会话 |
| `--from-pr 123` | 恢复与 GitHub PR 关联的会话 |
| `--remote "task"` | 在 claude.ai 创建远程 Web session |
| `--teleport` | 将 Web session 迁移到本地 terminal |
| `--chrome` | 启用浏览器集成（Web 自动化）|
| `--verbose` | 显示完整 turn-by-turn 输出 |

**System prompt flags 优先级**：`--system-prompt`（替换）互斥；`--append-system-prompt`（追加）可与任一组合。大多数场景用 append，保留内置能力。

---

## 三、斜杠命令（会话内）

### 上下文与 Token 控制

| 命令 | 说明 |
|------|------|
| `/context` | 实时查看 token 消耗和上下文窗口占用 % |
| `/compact` | 压缩对话历史为高密度摘要，释放上下文空间 |
| `/clear` | 彻底清空当前会话上下文（开启全新任务）|
| `/cost` | 查看本次会话 token 花费（美元）|
| `/memory` | 查看/管理 CLAUDE.md 已加载规则，手动添加跨 session 规则 |
| `/dream` | 记忆整合：将 Auto Memory 临时笔记提炼为高质量长期知识库资产 |

### 任务与工作流自动化

| 命令 | 说明 |
|------|------|
| `/plan` | 切换计划模式（等价 Shift+Tab）：先规划再执行 |
| `/ultraplan` | 超级计划模式：复杂重构/跨系统任务云端高性能容器深度分解 |
| `/loop` | 定时循环执行（`/loop 5m /foo`，默认 10m）|
| `/schedule` | 设置定时任务（结合 Routines）|
| `/init` | 自动扫描代码库生成 CLAUDE.md |

### Level 4+ 高级命令（Boris Cherny）

| 命令 | 说明 |
|------|------|
| `/btw` | 任务中提快问（不打断主流程，不污染 session 历史）|
| `/branch` | 在当前节点创建对话分叉（一个分支试一种方案）—— 对话级 Git |
| `/insights` | 分析过去一个月使用模式：高重复操作、token 浪费点、应升级为 Skill 的 prompt |
| `/output-style new` | 切换输出风格（内置：default/explanatory/learning；支持自定义如 code-reviewer/no-fluff）|
| `/focus` | 隐藏中间步骤，只显示最终结果 |
| `/rename` | 更改当前 session 显示名称 |
| `/resume` | 选择 session 恢复（交互式选择器）|

### 审查与调试

| 命令 | 说明 |
|------|------|
| `/rewind` | 回溯 checkpoint 重新总结 |
| `双击 ESC` | 回溯修改上一条输入 |
| `ESC` | 中止当前执行 |
| `!cmd` | 直接执行终端命令（如 `!git status`）|

---

## 四、权限模式速查

| 模式 | settings 值 | 行为 |
|------|------------|------|
| Ask permissions（默认）| `default` | 每次文件编辑/命令执行均提示确认 |
| Auto accept edits | `acceptEdits` | 自动接受文件编辑，命令执行仍提示 |
| Plan mode | `plan` | 只读探索 + 生成计划，不执行代码变更 |
| Auto mode | `auto` | 后台安全分类器验证，低风险自动放行（需 Team/Enterprise，Sonnet/Opus 4.6+）|
| Bypass permissions | `bypassPermissions` | 完全跳过所有提示（仅限沙盒/Docker）|

---

## 五、Effort 层级（Claude Opus 4.8 新增）

| 等级 | 适用场景 |
|------|----------|
| `max` | 极高复杂度任务（注意 diminishing returns 风险）|
| `xhigh` | 编码和 Agentic 最佳默认值 |
| `high` | 智能敏感型通用场景最低下限 |
| `medium` | 成本敏感，接受一定智能降低 |
| `low` | 短任务、延迟敏感（低复杂度）|

Effort 直接影响 tool call 频率、子 agent 生成数量、reasoning token 投入。低 effort 下模型字面遵守指令（不自动推广），适合精确结构化流水线。

---

## 关联实体

- [[Claude_Code_Commands]] — 高频命令心智模型与六大工作流
- [[Claude_Code_Settings]] — settings.json 权限规则配置
- [[Claude_Code_Hooks]] — 事件驱动的自动化层
- [[Claude_Code_Routines]] — 定时任务与 Cowork 自动化
- [[Claude_Code_Subagents]] — Subagent 与 Agent Teams 架构
- [[Prompt_Engineering_Advanced]] — Opus 4.8 effort 调优与提示工程
