---
title: Claude Code Advanced Features
aliases: ["Claude Code 高级功能", "Advanced Claude Code", "AI Native 开发"]
tags: [claude-code, advanced, features, claude-md, mcp]
category: claude-tooling
parent: "[[Claude_Code_Settings]]"
created: 2026-05-15
date: "2026-05-15"
---

# Claude Code Advanced Features（高级功能）

Parent: [[Claude_Code_Settings]]
Source: [Source: raw/Claude Code advanced features.md]

## 功能总览
Claude Code 高级功能将其从"编程聊天界面"升级为**"AI 原生开发的操作系统"**。

## 1. CLAUDE.md 项目记忆系统
存放 tech stack、coding style、run commands、business priorities。Claude 启动时自动读取，保持全项目一致性。
- 结构化写明：Your role / Tech preferences / Guardrails / Folder mappings
- 每天更新 2 次（早晚），添加新 Skills 或 API reference
- 用 `/extended thinking` 让 Claude 自动优化此文件
- 详见 [[CLAUDE_md_Best_Practices]]

## 2. Skills 与 Custom Slash Commands
Skills = 可复用 SOP 文件夹（`.claude/skills/skill-name/skill.md`），含 YAML front matter + 步步规则 + reference files + guardrails。
- Claude 通过 front matter 扫描（约 100 token）自动触发
- 支持 sub-agent 委托（如 heavy ClickUp search 交给专用 sub-agent）
- Global Skills：`~/.claude/skills`（全项目可用）
- Project Skills：本地项目目录
- 详见 [[Claude_Code_Skills]]

## 3. 权限模式 + Extended Thinking

| 模式 | 触发方式 | 适用场景 |
|------|----------|----------|
| Plan Mode | `Shift + Tab` | 复杂任务先规划后执行 |
| Auto Mode | 默认 | 安全动作自动跑，风险动作询问 |
| Bypass | 显式配置 | 全自动（信任后使用，仅限沙盒） |
| Extended Thinking | `/model` 切换 Opus | 多步深度推理，透明 chain-of-thought |

- 1M token context 支持超大 codebase 一次性分析
- 详见 [[Claude_Code_Settings]]

## 4. Computer Use + MCP/Hooks/Agents
- **Computer Use**（Pro/Max 计划）：point-click-navigate 屏幕、运行终端、调用工具
- **MCP 服务器**：动态加载工具，详见 [[MCP_Connectors]]
- **Hooks**：事件触发时自动执行，详见 [[Claude_Code_Hooks]]
- **Agent Teams**：Opus 协调多个 Sonnet 子代理并行工作，详见 [[Claude_Code_Subagents]]

## 5. Cloud Routines 与 Cadence 自动化
云端定时任务（Pro 5 次/天，Max 更高），laptop 关闭仍运行。
- 支持 schedule、GitHub event、API trigger
- 在 repo 中配置 routine prompt（明确 env var、full network access）
- 结合 Wiki ingest 自动更新知识库
- 详见 [[Claude_Code_Routines]]

## 6. Wiki 层 + Decisions Log（永久知识管理）
- `/raw` + ingest → `/wiki`（带 `_index/_log/_hot.md` 缓存）
- Decisions 文件夹 append-only 记录 reasoning
- 会议笔记/YouTube transcript 永久可搜，token 用量降 95%

## 7. Audit/Level Up（自我迭代）
- `/audit`：打 Four Cs 分数
- `/level-up`：五问发现 gaps
- Daily：早计划 + 晚复盘；Weekly：Audit + Level Up
- 系统自动进化：20% 初始 dip 后 50%+ 长期增益

## 快速上手规则（2026 年 5 月）
- Sonnet 日常，Opus 复杂任务
- 所有 key 放 `.env`，永不 chat 输入
- 先 POC 小测试，再规模化
- 生产环境用 separate AI 账号 + read-only key

## 8. 学术研究者工作流（Academic Researcher Workflow）

学术项目跨月份甚至数年积累，推荐以下组织模式：

**文件夹结构（论文项目示例）**：
```
My Dissertation/
├── CLAUDE.md           ← 全局"宪法"：项目概览 + 大背景（精炼，不超过 80 行）
├── Literature/         ← PDF + 已发表文献笔记
│   └── CLAUDE.md       ← 局部规则："只分析本文献批量，输出标注引用格式"
├── Chapters/           ← 各章草稿
│   └── CLAUDE.md       ← 局部规则："匹配我的学术写作风格，不改变论证结构"
├── Data/               ← 数据集
│   └── CLAUDE.md       ← 局部规则："数据分析时引用具体行号，禁止推断"
├── Notes/              ← 会议记录 + 随手想法
└── Correspondence/     ← 导师邮件、审稿意见
```

**两层 CLAUDE.md 分工**：
- **全局 CLAUDE.md**（项目根目录）：项目定义、研究问题、学科规范、永久约束
- **局部 CLAUDE.md**（各子目录）：子任务专属规则，防止通用规则与特定任务冲突

**为什么要分层**：让 Claude Code 在 `Chapters/` 里专注写作风格，在 `Data/` 里专注数据精度——同一个 agent，不同子目录给出不同专家水准的输出。（类比：你不会给研究助手同一份"文献综述指令"和"数据清洗指令"）

*[Source: raw/Claude Code for Academic Researchers.md]*


- [[CLAUDE_md_Best_Practices]] — 核心记忆文件
- [[Claude_Code_Skills]] — Skills 系统
- [[Claude_Code_Hooks]] — 自动化钩子
- [[Claude_Code_Routines]] — 定时任务
- [[Context_Engineering]] — 上下文压缩机制

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图

## 9. 大型代码库企业部署模式（官方 2026-05-14）

### Agentic Search vs RAG
Claude Code 像工程师一样工作：遍历文件系统 + grep 搜索 + 实时代码库（不构建索引）。RAG 的失败模式（索引过时）不存在，但代价是依赖足够的初始上下文。

### 7 组件 Harness 优先级
| 组件 | 加载时机 | 最大误区 |
|------|----------|----------|
| CLAUDE.md | 每个 session | 把可复用专长塞进来（该放 Skills） |
| Hooks | 事件触发 | 用 prompt 代替 Hooks 做自动化 |
| Skills | 按需加载 | 把所有内容塞进 CLAUDE.md |
| Plugins | 始终可用 | 让好配置停留在部落知识 |
| LSP | 始终可用 | 以为会自动生效（需手动安装） |
| MCP Servers | 始终可用 | 跳过基础配置直接接 MCP |
| Subagents | 按需调用 | 在同一 session 同时探索和编辑 |

**关键洞见（Anthropic 官方）**：Harness 决定 Claude Code 性能，比模型本身更重要。

### 大型代码库导航三法
1. **分层 CLAUDE.md**：根目录只放大局指针 + 关键 gotchas；子目录放本地规范；随目录遍历自动加载
2. **从子目录初始化**（非 repo root）：Claude 会自动上行加载所有 CLAUDE.md，根目录上下文不会丢失
3. **LSP 集成**：给 Claude 符号级导航（"go to definition"/"find all references"），避免 grep 文本匹配带来的歧义

### 组织治理建议
- **DRI 模式**：指定 1 人或小团队管理 CLAUDE.md、Plugins、MCP 配置
- **跨职能工作组**：工程 + 信息安全 + 治理三方联合制定 rollout 路线
- **配置审查周期**：每 3-6 个月或重大模型版本后做一次（旧规则可能限制新模型能力）

*[Source: raw/How Claude Code works in large codebases_ Best practices and where to start.md]*

## 10. Anthropic 内部工作流（7 步循环）

```
1. Interview → Claude 用 AskUserQuestion 提问，写出 SPEC.md
2. Fresh session → 基于 spec 做实现规划（新对话=无偏见）
3. Execute → Plan Mode 下写代码
4. Review → 另开 session 评审（同 session 写者偏向辩护代码）
5. Challenge → Subagents 挑战评审结论（过滤假阳性）
6. Commit → 每个逻辑变更单独一个 commit
7. Update CLAUDE.md → "让这个错误不再发生"= Claude Code 最强 prompt
```

**Writer/Reviewer 分离原则**：  
```bash
# Session 1: 写代码
claude -p "implement payment flow based on SPEC.md"

# Session 2: 评审（fresh context）
claude -p "review last commit for bugs, security, edge cases. Be critical."
```
同一 session 写出的代码，评审时会偏向辩护。分开 session 才能得到真实批评。

**Skills-as-Folders 模式（Anthropic 内部）**：
```
.claude/skills/frontend-design/
├── SKILL.md           → 核心指令
├── references/        → colors.md / typography.md / components.md
├── assets/            → template.html
└── examples/
    ├── good-card.html → 示范
    └── bad-card.html  → 反例
```
最有价值的 section：**Gotchas**（基于真实问题的常见错误总结）。

**Skill 四类型**：Library（SDK 用法）/ Verification（带脚本的测试）/ Workflow（deploy/migrate 流程）/ Style（设计+代码规范）  
其中 **Verification Skills 价值最高**：含脚本、视频截图验证、每步断言。

*[Source: raw/The Claude Code Setup Behind Anthropic's Own Engineers (Exact Config You Can Copy).md]*

---

## §11. Remote Control（跨设备会话）

允许从手机/平板/其他浏览器继续本地Claude Code会话，处理仍在本地机器上运行。

**启动命令**：`claude remote-control --name "My Project"`

**关键Flag**：
- `--spawn same-dir`：所有远程会话共享当前工作目录
- `--spawn worktree`：每个按需会话获得独立隔离的git worktree

**默认启用**：在 `/config` 中设置 "Enable Remote Control for all sessions" 为 `true`

**移动端访问**：服务器模式下按空格键显示QR码，快速手机连接。

**典型工作流**：
- 公司电脑：完整运行Harness
- iPhone Termius：远程连接后审查 plans/ 和 artifacts/，输入 `/approve` 指令

*[Source: raw/Claude Code Extract.md]*

---

## §12. 上手心智模型：把 Claude Code 当新入职的优秀工程师

[Source: raw/Claude Code Foundamentals.md]

**错误开场**：立刻让它写大功能 → 跑偏/输出差。
**正确开场**：先从 Codebase Q&A 开始，理解后再动手。

| 阶段 | 操作 | 类比 |
|------|------|------|
| 1. 探索 | 问代码库问题（"这个函数如何被调用？"）| 新员工读代码 |
| 2. Brainstorm | 先要求列出方案 + 计划，再确认 | 一起讨论需求 |
| 3. 确认 | 审查计划后才允许执行 | 批准设计文档 |
| 4. 实现 | Claude 按计划分步执行 | 工程师写代码 |

**Anthropic 内部数据**：从这种方式开始，技术 onboarding 从 2-3 周缩短到 2-3 天。

### 上下文给法：少即是多

- `CLAUDE.md` 放真正需要每次都记住的内容（风格指南、关键架构决策、常用命令）
- 子目录放局部 `CLAUDE.md`（模块专属规则）
- 其余内容按需通过 Skills 或 @mention 文件加载
- 避免"上下文膨胀让模型变笨"——不是越多越好

### 工具自组合原则

只需要告诉 Claude 目标，它会自动组合工具（读文件/写文件/Bash/搜索）去完成，甚至可以说"用这个 CLI 帮我做某某事"，它会自己看 `--help` 再执行。团队常用工具（Puppeteer/自定义 CLI/MCP server）告诉它一次，以后自动使用。