---
title: CLAUDE.md Best Practices
aliases: ["CLAUDE.md 最佳写法", "项目交接文档", "上下文规则文件"]
tags: [claude-md, context, rules, best-practice, harness]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# CLAUDE.md Best Practices

Parent: [[index]]

> 核心论点：CLAUDE.md 是 Claude Code 的"职场说明书"，不是对话记录。它必须**短小精炼**（60-80 行以内），只写"去掉这行 Claude 就会犯错"的内容。

---

## 三级配置架构

| 层级 | 路径 | 内容 |
|------|------|------|
| Global | `~/.claude/CLAUDE.md` | 个人永久偏好（所有项目自动叠加）|
| Project | `项目根目录/CLAUDE.md` | 团队标准 + 技术约束 |
| Local | `~/.claude/local/CLAUDE.md` | 个人小习惯 |
| Subdir | `子目录/CLAUDE.md` | 模块级细化 |

**加载顺序**：Global → Project → Local（离工作目录越近，优先级越高）

---

## 七段核心结构模板

```markdown
# CLAUDE.md

## Project Overview
[2-3 句：项目做什么、给谁用、核心优先级]

## Tech Stack
- Framework: [Next.js 14 App Router]
- Package manager: [pnpm]
- Database: [Supabase]

## Critical Commands
- build: pnpm build
- test: pnpm vitest
- lint: pnpm lint --fix
- dev: pnpm dev

## Hard Rules (IMPORTANT)
- YOU MUST: 任何改动前先输出 Plan 并等我确认
- NEVER: 不要重写整个文件，只改指定部分
- IMPORTANT: 永远不要 commit .env 或任何密钥
- NEVER: 不要使用 npm，用 pnpm
- YOU MUST: 改完后自动跑测试并输出 git status

## Architecture Map
- src/: 核心业务逻辑
- components/: 可复用 UI
- lib/: 工具函数

## Workflow Preferences
- 先输出 Plan（改哪些文件、为什么、影响范围）
- 输出格式：bullet points + 代码块
- 完成后验证构建和测试通过才算完成

## What NOT to include
- 不要重复已记在 memory/ 里的内容
- 不要加"think step by step"或性格描述
```

---

## 写作铁律

1. **控制在 60-80 行**：超过 150 instructions Claude 会丢规则
2. **只写负面规则**：`NEVER`、`IMPORTANT`、`YOU MUST` 开头，每条针对具体错误
3. **删通用描述**：`"be a senior engineer"` 等 Claude 自带能力的描述全部删掉
4. **不写 Claude 已学会的内容**：`/memory` 查看已记内容，避免重复

---

## 自我维护循环

```
Claude 犯错后立即输入：
"Update CLAUDE.md so you don't make this mistake again."

每月 review 一次，删掉无效规则，保持 <80 行。
```

---

## 辅助文件（超出 CLAUDE.md 的内容）

| 文件 | 内容 |
|------|------|
| `[[AI_Team_Coding_Practice|AGENTS.md]]` | Agent 工作方式、构建测试命令、工具发现 |
| `[[AI_Team_Coding_Practice|DECISIONS.md]]` | 架构选择、被拒绝方案、已知 bug 模式 |
| `.claude/rules/` | 路径/语言规则 |
| `.claude/skills/` | 工作流 Skill 文件 |

---

## Karpathy 12 规则系统（[[Claude_Code_Self_Evolving|Karpathy 四规则]] + 2026年5月扩展）

**实测结果**：原始 4 规则将 Claude 错误率从 41% → 11%（30 个代码库、6 周测试验证）。完整 12 规则覆盖到 May 2026 的 agent 编排问题。

**合规上限**：CLAUDE.md 超过 200 行时，Claude 遵守度急剧下降；超 4000 tokens 降至 30%。目标：65 行以内，12 规则。

### 原始 4 规则（January 2026，Forrest Chang 整理自 Karpathy 原帖）

```markdown
Rule 1 — Think Before Coding
  不做任何隐含假设，明确陈述假设，向用户呈现权衡，请求澄清再猜测，主动提出更简单方案。

Rule 2 — Simplicity First
  只写解决问题所需最少代码，不添加推测性功能，不为一次性代码创建抽象，
  如果资深工程师认为过于复杂必须简化。

Rule 3 — Surgical Changes
  只修改完成任务必须改动的代码，禁止"改进"未损坏的相邻代码/注释/格式，
  所有修改严格匹配现有代码库风格。

Rule 4 — Goal-Driven Execution
  不给执行步骤，给明确的成功标准，Claude 循环迭代直到验证标准已满足。
```

### 2026年5月扩展8规则（agent 编排场景）

```markdown
Rule 5 — Don't Make the Model Do Non-Language Work
  确定性逻辑（是否重试 API、如何路由消息、何时升级）写在代码中，不让 Claude 随机决定。

Rule 6 — Hard Token Budgets, No Exceptions
  每个任务设 token 上限，防止 agent 在死循环中耗尽上下文。
  无预算 = 模型不断迭代同一错误，最终重复提出早已被拒绝的方案。

Rule 7 — Surface Conflicts, Don't Average Them
  代码库存在两种矛盾模式时，要求 Claude 明确指出冲突，不"平均化"迎合两者。

Rule 8 — Read Before You Write
  修改代码前必须先理解相邻代码，防止添加与现有函数功能重复但冲突的实现。

Rule 9 — Tests Are Not Optional, But They're Not the Goal
  防止模型为"让测试通过"而写肤浅代码（如返回常数骗过测试），测试必须有意义。

Rule 10 — Long-Running Operations Need Checkpoints
  多文件重构或跨多个 Commit 的任务必须设检查点，
  无检查点 = 第 4 步出错后，第 5、6 步基于错误状态继续，回滚成本指数增长。

Rule 11 — Convention Beats Novelty
  即使有"更好"的新模式，也遵循项目现有规范。不在旧 React 类组件项目中强行引入 Hooks。

Rule 12 — Fail Visibly, Not Silently
  任何跳过或异常必须显式推送到输出端。
  静默成功（数据库迁移显示"成功"但跳过 14% 数据）= 最严重的失败。
```

沟通类：`Kill the filler`（无废话开头）/ `Always show options`（重大任务先给 2-3 方案）/ `Match length`（回复长度匹配任务需求）

行为类：`Ask before big changes` / `Stay focused`（只改要求部分）/ `Never act without asking`（外部操作必须 yes）

---

## 完整 21 规则系统（3 层结构 + ROI 数据）

**实际收益数据（@adiix_official，52 周追踪）**：添加前每周浪费 18 小时（$2,700/周，$140K/年）；添加后清理成本趋近 $0，年度价值恢复 $147K。

### 层级结构

| 层 | 文件 | 职责 |
|----|------|------|
| Layer 1 | CLAUDE.md（项目根目录，<500 词）| 21 条规则，自动加载，会话起点 |
| Layer 2 | MEMORY.md（项目根目录，<200 行）| 决策日志，每次会话主动读取 |
| Layer 3 | ERRORS.md（项目根目录）| 失败日志，类似任务前先检查 |

**ERRORS.md 使用规则**：某方案超 2 次尝试才成功时，记录 What failed / What worked / Note for next time。每次执行类似任务前先检查此文件。这是防止 AI 在同一陷阱反复跌倒的关键机制，[[Karpathy_Methodology]] 体系中未明确的工程补充。

### Part 1：Kill the Noise（7 条）
规则 5-11 在 Karpathy 4 条基础上消除每天 30 分钟重复上下文解释：
- **Kill the filler**：不以"好问题！"开头，直接给答案
- **Match length**：简单问题简短回答，复杂任务详细输出
- **Show options**：重大任务先给 2-3 方案再执行
- **Admit uncertainty**：不确定时明说，不造可信信息
- **Who I am**：写入角色/强项/学习中的领域
- **Current project**：写入项目目标/技术栈/禁用列表
- **Lock voice**：写作风格 + 禁用词汇

### Part 2：Stop Unauthorized Changes（7 条）
规则 12-18 阻止最常见的"自作主张"行为（每次意外重构 $450/周 损耗）：
- **规则 12（最重要）**：`Do not touch it. Ever.` — 这句话阻止 70% 的未授权重构，语言要用命令式而非建议式
- 变更前列 `Files changed` / `What modified` / `Files intentionally not touched`
- 破坏性操作（删除、覆盖、发布、push）必须当前消息内显式确认（"你之前说过"不算）
- 生产环境特殊 stop：deploy/migrate/API call/不可逆命令必须明确 in-session 确认

### Part 3：Real Memory Architecture（6 条）
规则 19-24 实现持久化决策记忆：
- `session end` 关键词触发 MEMORY.md 自动摘要写入
- `Permanent facts` 节：永久约束，任何任务冲突时先 flag
- 锁定 Stack：语言/框架/包管理器/数据库/测试/样式 — 不主动建议替代
- 扩展思考（Extended thinking）用于架构、性能权衡、数据库设计

**MEMORY.md 上限**：200 行，每周 review 精简。超限时让 Claude 自动压缩（保留当前计划 + 关键错误修正）。

*[Source: raw/How Karpathy's CLAUDE.md made me $147,000.md]*

---

## 关联实体

- [[Claude_Code_Hooks]] — CLAUDE.md 规则的确定性执行层
- [[Claude_Code_Skills]] — 从 CLAUDE.md 抽离的工作流封装
- [[Agent_Harness_Engineering]] — CLAUDE.md 在六层架构中的位置（Layer 1 上下文资产）
- [[Claude_Code_Settings]] — settings.json 与 CLAUDE.md 的权限边界
- [[Claude_Code_Self_Evolving]] — /evolve 自动把 correction 升级为 Hard Rule
- [[Instruction_Sharing]] — 多项目共享 Copilot 指令文件的 symlink/junction 方案（对比 Claude Code 三级分层）
- [[GBrain_Architecture]] — CLAUDE.md 是 GBrain"Compiled Truth"在单项目粒度的最小形态；GBrain 是 CLAUDE.md 扩展到 100k 页知识图谱的全量形态

*[Source: raw/Claude.md 最佳写法.md, raw/Claude Code 系统治驭工程指南.md, raw/Claude.md.md, raw/Karpathy methodology.md, raw/Karpathy's 4 CLAUDE.md rules cut Claude mistakes from 41% to 11%. After 30 codebases, I added 8 more.md]*

---

## Anthropic & Shopify 内部 CLAUDE.md 模式（2026年5月）

### Anthropic 内部模板（Boris Cherny）

```markdown
## Project
[项目描述]

## Principles
- Make minimal changes. Don't refactor unrelated code.
- Prefer simple, readable solutions over clever ones.
- Always run tests after changes. Fix failures before moving on.
- Ask before making architectural decisions.
- When unsure between approaches, explain both and let me choose.

## Workflow
- Plan before executing on anything non-trivial.
- Create separate commits per logical change.
- Commit messages: type(scope): description under 50 chars
- Run type check after every code change.

## What NOT to do
- Never commit .env files or secrets
- Never modify CI/CD without asking
- Never bypass type checking
- Don't add dependencies without discussing first
```

**删除测试（Anthropic 官方原则）**：每行都问"去掉这行，Claude 会犯错吗？"答案是否 → 删掉。5,000-token CLAUDE.md 每轮消耗 5,000 token。

### Shopify 内部模板（提交到 git 全团队共享）

```markdown
## Stack
Ruby on Rails, React, GraphQL, MySQL

## Commands
- Dev: `dev up && dev server`
- Test: `dev test [path]`
- Lint: `dev style`
- Type check: `bin/srb tc`

## Architecture
- app/models/ → ActiveRecord models, business logic
- app/controllers/ → thin controllers, delegate to services
- app/services/ → service objects for complex operations

## Rules
- NEVER bypass Sorbet type checking
- All new code must have type signatures
- Database queries only through established patterns
- IMPORTANT: run `dev test` after every change
```

**Shopify 教训**：把所有标准和 convention 全塞进 CLAUDE.md 会拖慢性能（每轮都要付出 token 成本），保持精简 + 分层。

### Interview Pattern（Anthropic 工程师标准流程）

```
I want to build [brief description].
Interview me in detail using the AskUserQuestion tool.
Ask about technical implementation, UI/UX, edge cases,
concerns, and tradeoffs. Don't ask obvious questions,
dig into the hard parts I might not have considered.
Keep interviewing until we've covered everything,
then write a complete spec to SPEC.md.
```

**关键**：spec 写完后**另开新 session** 执行（新上下文 = 无规划阶段偏见）。

*[Source: raw/The Claude Code Setup Behind Anthropic's Own Engineers (Exact Config You Can Copy).md, raw/The Claude Code Setup Behind Shopify's 23,000 Engineers (Exact Config You Can Copy).md, raw/How Claude Code works in large codebases_ Best practices and where to start.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图
- [[Karpathy_Methodology]] — Karpathy 完整方法论（4+8 规则体系详解）