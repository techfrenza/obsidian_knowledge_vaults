---
title: Claude Code Subagents
aliases: ["子代理", "Subagent 上下文隔离", "Fork 继承"]
tags: [subagents, claude-code, context-isolation, fork, worktree]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Code Subagents

Parent: [[index]]

> 核心论点：Subagents 解决的核心问题是**上下文污染**——将高噪声的探索过程隔离在子窗口，主线程只接收精炼摘要。

---

## 创建位置（优先级从高到低）

- 项目级（团队共享）：`.claude/agents/`（git 提交）
- 全局个人：`~/.claude/agents/`（所有项目自动生效）
- 同名时更高优先级覆盖

---

## 自定义 Subagent 模板

```markdown
---
name: code-reviewer
description: 审查控制器文件，找出安全问题和性能瓶颈
model: claude-4-sonnet
tools: [grep, read, ls]
---

[后面写系统 prompt]
```

---

## 内置 Subagents

| Agent | 用途 |
|-------|------|
| **Explore** | 代码搜索（grep/find/ls），返回发现，主上下文不留痕迹 |
| **Plan** | 读文件、理解架构，只返回 step-by-step 文档 |

使用方式：在主会话直接说 "用 Explore 找所有用户认证相关代码"

---

## Fork 上下文继承

已花 100k tokens 理解 codebase 时必用：

```bash
# 永久开启
export CLAUDE_CODE_FORK_SUBAGENT=1   # 加入 ~/.zshrc
# 临时单次
/fork
```

效果：子代理继承父会话完整上下文 + prompt cache（后续子 agent 输入 token **便宜 10 倍**）

---

## 配置最佳实践

```yaml
tools: [只给必要工具]
model: claude-haiku-4-5  # 探索用 Haiku，审查用 Opus
maxTurns: 10
isolation: worktree     # 动文件时隔离
```

**反模式避免**：
- 权限别和主线程一样宽
- 输出格式必须固定（子代理返回 ≤2000 token condensed summary）

---

## 日常工作流程

1. **长任务前**："用 Explore 先查一遍" → 只返回 3 行结果，上下文保持干净
2. **复杂重构**：用 Plan 子 agent → 主窗口只看到最终 plan
3. **继承现有理解**：先设 fork 变量或 `/fork`
4. **高输出任务**：扔 Subagent（扫代码、跑测试、审查）
5. 后台长命令：`Ctrl+B` 移后台，用 BashOutput 工具查结果

---

## Parallel Agents 工作流

同时开 3-10 个独立标签页，每个有独立上下文：
```
Session 1：实现 Feature A
Session 2：写 Feature B 的测试和文档
Session 3：数据库迁移
Session 4：重构 auth 模块
Session 5：调查生产 bug
```
循环审查，只在需要决策时介入——这是**编排**而非**多任务**。

---

## /agents 命令：Subagent 管理界面

`/agents` 打开交互式选项卡界面：
- 浏览所有代理：内置（Explore/Plan）、个人全局（`~/.claude/agents/`）、项目（`.claude/agents/`）、插件
- 优先级：CLI 参数 > 项目级 > 个人级 > 插件级，同名时高优先级覆盖
- 操作：创建（AI 自动生成 or 手填）、编辑（`e` 键开编辑器）、删除
- 快速列表（非交互）：`claude agents`

---

## SKILL.md 与 Subagent 集成

`context: fork` 让技能在隔离子代理中运行：

```yaml
---
name: security-auditor
description: 审查控制器文件安全漏洞。用户说"检查安全"或"审计代码"时触发。
context: fork
agent: Explore
model: haiku
allowed-tools: Read, Grep, Glob
---

你是安全审计子代理。审查 $0 目录下的代码。
步骤：1. grep SQL 注入模式 2. 检查 API 端点认证 3. 按严重程度排序输出
```

关键字段：`context: fork`（必选）、`agent`（模板）、`allowed-tools`（最小权限）、`$ARGUMENTS/$N`（变量）

---

## 7 个生产级 Sub-Agent 角色模板（直接复制）

部署位置：`.claude/agents/<name>.md`（项目级）或 `~/.claude/agents/<name>.md`（全局）

每个 .md 文件结构：
```
Job:   角色 + 薪资对标（让 Claude 知道它扮演谁，对标预期）
Brain: 专属思维方式 + 输出格式（结构化 brief、SOP、计算等）
Rules: 严格约束（不污染 main thread、只返回指定格式、独立 context window）
```

| Agent | 输入 | 输出格式 | 对标价值 |
|-------|------|---------|---------|
| **Researcher** | 任意主题 | 3 findings + 3 contradictions + 3 open questions | $80K/年研究员 |
| **Editor** | 草稿 | 砍掉 30% 字数 + 强化 hook + 标记弱论点 | $60K/年编辑 |
| **Project Manager** | 目标 | 周计划 + owner + deadline + single point of failure | $90K/年 PM |
| **Analyst** | CSV/数据 | story + outliers + "so-what" + 建议图表 | $70K/年数据分析 |
| **Recruiter** | 职位描述 | sourcing plan + outreach template + screening rubric + rejection email | $80K/年招聘 |
| **Ops Lead** | 流程描述 | 3步可自动化 + 2步可删除 + 1步永不动 + SOP | $85K/年 Ops |
| **CFO** | 财务数字 | runway + burn + 最大出血点 + 第一刀砍哪 | $150K/年 CFO |

**启动顺序建议（按角色）**：
- Solo founder：CFO + Project Manager + Recruiter
- Engineer/builder：Researcher + Analyst + Ops Lead  
- Content creator：Researcher + Editor + Analyst

**关键约束**：每个 Agent 保持独立 context window，不污染主线程。返回格式固定（≤2000 token structured output），方便 Orchestrator 消费。

*[Source: raw/The 7 Claude Sub-Agents That Replace a $200K Team.md]*

---



- [[Agent_Harness_Engineering]] — Subagent 在 Harness 架构中的位置（Layer 5）
- [[Claude_Code_Skills]] — Skill 处理工作流，Subagent 处理隔离执行
- [[Claude_Code_Commands]] — `/fork`、`Ctrl+B` 等命令
- [[MCP_Production_Agent]] — Context-Efficient 模式与 subagent 组合（Fork 后 prompt cache 的 10x 成本优势）
- [[Opus_4_7_Migration]] — 4.7 默认子代理变少，必须主动要求 fan out；xhigh effort 与 Subagent 调度策略
- [[Anthropic_Agent_SDK]] — SDK 层面的子代理系统（扁平层级、上下文隔离、并行化原则）
- [[Multi_Agent_Architecture]] — 三层架构中 Agents 层的 Handoff 模式与安全分层隔离；4-Agent 团队蓝图（Research/Production/Quality/Distribution）
- [[Claude_Code_Hacks]] — Hack #11/#13: 并行 subagent + Haiku 路由策略

*[Source: raw/Claude Code Subagents context.md, raw/Claude Code Subagent.md]*

---

## Subagents vs Agent Teams：精确区分

来源：@akshay_pachaar — Claude Subagents vs. Agent Teams, explained（2026-03-15）

**两种范式解决完全不同的问题：**

| 维度 | Sub-agents（子代理） | Agent Teams（Agent 团队） |
|------|---------------------|--------------------------|
| 生命周期 | 短暂，完成任务即消失 | 持久，跨 Session 保持状态 |
| 通信 | 只向父代理报告（单向） | 点对点直接通信（多向） |
| 协调 | 父代理是唯一协调者 | 有 Team Lead + 共享任务列表 |
| 记忆 | 无跨 Agent 共享状态 | 有共享状态，发现可立即通知队友 |
| 适用 | 独立并行任务 | 需要持续协商的任务 |

**Sub-agents 的核心是压缩，不仅仅是并行**：子代理将大量探索压缩成干净信号，不污染父代理上下文。

**Agent Teams 的 blockedBy 字段**：shared task list 做真实协调工作——不需要 Lead 手动管理顺序，test-writer 在 backend-dev 完成前自动等待。

### 上下文中心设计原则（非角色中心）

**错误直觉**：按角色分拆（Planner/Implementer/Tester）。
**问题**：每个移交都丢失信息（类电话游戏），质量在边界处下降。

**正确思路**：问"这个子任务真正需要什么上下文？"
- 两个子任务需要深度重叠信息 → 同一个 Agent
- 两个子任务可以用干净接口隔离 → 分开

**具体案例**：实现某功能的 Agent 应该也负责写该功能的测试（它已有上下文）；分拆成两个 Agent 创造的移交成本大于并行收益。

### 五种编排模式

| 模式 | 描述 | 适用 |
|------|------|------|
| Prompt chaining | 顺序步骤，每步处理前步输出 | 顺序依赖 |
| Routing | 分类器决定哪个专项处理器 | 成本优化（难 → 强模型，易 → 弱模型）|
| Parallelization | 独立子任务同时运行 | 相互独立的探索/搜索 |
| Orchestrator-worker | 中央 Agent 分解/委托/综合 | 生产系统主流架构 |
| Evaluator-optimizer | 一个生成，一个评估，循环 | 质量比速度重要时 |

### 何时不用多 Agent 系统

**三种值得用的情况**：
1. 上下文保护（子任务信息不应污染主线程）
2. 真正的并行化（独立探索受益于同时覆盖）
3. 专门化（任务需要相互冲突的系统 Prompt）

**编码特别警告**：并行 Agent 写代码会做出不兼容的隐性假设。合并时冲突难以 debug。Sub-agents 写代码时只应用于探索和回答，不应与主 Agent 同时写代码。

**最重要的设计原则**：从一个 Agent 开始，推到它失败为止。失败点告诉你下一步精确地需要添加什么。

*[Source: raw/Claude Subagents vs. Agent Teams, explained.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图