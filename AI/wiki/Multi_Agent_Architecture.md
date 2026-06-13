---
title: Multi-Agent Architecture（多 Agent 三层架构）
aliases: ["多Agent架构", "三层Agent架构", "Skills+Orchestrator+Subagent"]
tags: [multi-agent, architecture, skills, orchestrator, subagent]
category: architecture
parent: "[[Enterprise_AI_Architecture]]"
created: 2026-05-15
date: "2026-05-15"
---

# Multi-Agent Architecture（多 Agent 系统三层架构）

Parent: Enterprise_AI_Architecture
Source: [Source: raw/Anthropic MCP Connectors.md（三层架构部分）; raw/Anthropic MCP Connectors (1).md]

## 三层核心分离（立即可复制的架构模板）

### Skills 层（可复用领域专长）
- **单源权威**：所有 Skill 只在一个规范库中编写（按垂直领域分组），通过自动化 sync 脚本同步到每个 Agent
- **必须被动**：只描述"如何做好 X"，绝不包含"先做 A 再做 B"的流程逻辑
- **实现方式**：用 Slash Command 暴露（如 `/comps`、`/dcf`），让用户或 Agent 按需调用
- **漂移保护**：构建时运行 drift linter，有 Skill 漂移则直接失败（CI 强制一致性）
- 详见 [[Claude_Code_Skills]]

### Agents 层（工作流编排器）
固定 5 段 Prompt 模板（严格顺序）：
1. **Frontmatter**：元数据 + 允许工具列表
2. **Identity**：我是谁，我负责什么
3. **Deliverables**：精确输出物清单（例如"Break list + Root-cause trace + Exception report"）
4. **Workflow**：编号步骤，每步引用具体 Skill 名称
5. **Guardrails**：负面清单，绝不做的事

### MCP Connectors 层（集中数据接入层）
- 在核心插件中维护单个 `.mcp.json`，所有 Agent 继承
- 改数据源只需改一个文件，无需碰 Skill 或 Agent
- 详见 [[MCP_Production_Decision_Framework]]

## 安全分层隔离（处理不可信文档的核心模式）

```
不可信文档 → Reader Tier → Orchestrator Tier → Resolver Tier
                                                      ↑
                                               Critic（独立验证）
```

| 层级 | 权限 | 说明 |
|------|------|------|
| Reader Tier | 只读不可信文档，无 MCP，无写权限 | 输出必须 schema 验证 + 长度限制 + 字符白名单，防止 prompt injection |
| Orchestrator Tier | 不碰原始文档 | 聚合 Reader 输出，调用 MCP 读可信系统 |
| Resolver Tier | 可写文件，无 MCP 访问，无外部通信 | 只处理已验证数据 |
| Critic | 独立验证 | 插入 Resolver 之前，防止幻觉穿透 |

**工具配置原则**：deny-by-default（YAML 中默认 `enabled: false`，只显式开启需要的工具）

## Agent 间 Handoff 模式
- 输出结构化 `handoff_request`
- 由外部编排器（Python / Temporal / Airflow，见 [[LangGraph_Deep_Agents]]）校验 allowlist 后路由
- 避免 Agent 自行转发导致权限越界

## 双模式部署
同一套 Skill + Agent Prompt：
- **交互模式**：人类在环（见 [[Human_In_The_Loop]]）
- **Headless 模式**：加一层 `agent.yaml`（注入环境 MCP URL + "输出到 ./out/"），实现 API 自动运行（见 [[Claude_Code_Routines]]）

## Agent Prompt 模板（直接复制）
```
Frontmatter: name, description, tool allowlist

Identity: 你是 GL Reconciler，负责...

Deliverables:
- Break list：每条 GL 差异（账户、余额、差异、疑似原因）
- Root-cause trace：交易级证据
- Exception report：控制器签批格式

Workflow:
1. 确认范围 → 调用 sector-overview skill
2. 拉数据 → 使用 CapIQ MCP
3. 建模 → 调用 dcf-model skill

Guardrails:
- 绝不直接写 journal entry
- 所有数字必须可追溯，否则标记 [UNSOURCED]
- 关键检查点必须人类审批
```

## 4-Agent 团队蓝图（Content/Knowledge Work 通用模板）

来源：《Claude Multi-Agent Systems - The Complete Guide》

**核心原则**：专家团队永远优于通才独自工作。单个 Claude 实例跨 Research/Writing/Review/Distribution 四阶段输出平庸；四个专化 agent 并行输出卓越。

```
Agent 1 — Research Agent
  Input:  主题 / 问题 / Brief
  Output: 结构化 Research Brief
  禁止:   写作、编辑、发布

Agent 2 — Production Agent
  Input:  Research Agent 的 Brief
  Output: 完整初稿
  禁止:   研究、编辑、发布

Agent 3 — Quality Agent
  Input:  Production Agent 的初稿
  Output: 审批稿 OR 具体修改 Brief（二选一）
  禁止:   研究、从头写作、发布

Agent 4 — Distribution Agent
  Input:  Quality Agent 审批的稿件
  Output: 按目标平台格式部署的内容
  禁止:   研究、写作、质量评估

Orchestrator
  Input:  初始任务
  Output: 完整交付物
  知道:   所有 agent 的工作状态
  其他 agent: 只知道自己的任务
```

**并行化价值**：4 agent 并行运行四阶段 vs 单 agent 串行跑四阶段 → 速度提升 4x。内容运营场景下（每周 20 篇），并行化差异本身即值得引入架构。

*[Source: raw/Claude Multi-Agent Systems - The Complete Guide.md]*

---


- Claude_Code_Skills — Skills 层的设计与实现
- MCP_Production_Decision_Framework — MCP Connectors 层的生产决策
- Human_In_The_Loop — Guardrails 中的人工审批机制
- [[Anthropic_Agent_SDK]] — Agent 的底层 SDK
- Enterprise_AI_Architecture — 企业级 AI 架构全图
- [[Claude_Code_Security]] — deny-by-default 工具配置与安全分层的代码层实现
- [[Production_Reliability_MOC]] — 安全分层隔离是生产可靠性三大支柱之一

---

## Factory Missions 系统（长期多 Agent 任务架构）

来源：Factory CEO Luke Harries《Multi-Agent 工程系统》（2026年演讲）

> **核心论断**：软件工程的瓶颈已不是智能，而是**人类注意力**。Missions 解耦"人类定义要什么"与"系统自主执行多天"。

### 五种多 Agent 协作模式 Taxonomy

| 模式 | 描述 | 适用场景 |
|------|------|---------|
| **Delegation** | 父 Agent 派生子 Agent | 需要专领域处理（最常见） |
| **Creator-Verifier** | 独立 Agent 审查，消除 sunk cost bias | 代码审查、质量保障 |
| **Direct Communication** | Agent 间直接聊天（高风险：状态碎片化） | 小规模，需共享状态管理 |
| **Negotiation** | 多 Agent 围绕共享资源协商正和解法 | 同模块并发修改 |
| **Broadcast** | 单 Agent 向全体推送约束/计划更新 | 长期任务保持对齐 |

### Missions 三角色架构

```
Orchestrator（指挥官）
   → 对话中 scope 需求 → 输出 Plan + Validation Contract（先定义"什么叫 Done"）
         ↓
Workers（工人）
   → 接受干净上下文 → 实现具体特性 → Git Commit → 填写结构化 Handoff
         ↓
Validators（验证者）
   → Scrutiny Validator（test/lint/独立 Code Review Agent）
   → User Testing Validator（Computer Use，真实点击验证产品可用性）
```

**最大创新**：Validation Contract 在**规划阶段**提前写好（可能包含数百条独立断言），而非代码写完后补测试。

### 结构化 Handoff（防止信息丢失）

每个 Worker 完成后必须填写：
- 完成了什么 / 留下什么问题
- 运行过哪些命令 + exit code
- 是否遵守 Orchestrator 定义的流程

里程碑边界检查 → 发现未解决问题 → 自动创建 Follow-up Features（自愈机制）

### 并行策略：Serial 主干 + 局部并行

- **串行**：Feature 层（保证代码库演化连续可理解）
- **并行**：只读操作（代码搜索、API调研、Code Review）
- 最长 Mission 已跑 16 天；目标 30 天

### Droid Whispering（模型分配原则）

| 角色 | 模型特性 | 理由 |
|------|---------|------|
| Orchestrator | 慢思考强模型 | 战略拆解 + 约束设计 |
| Worker | 代码流畅、创造力强 | 快速落地 |
| Validator | 精确遵循指令 | 严格按验收契约检查 |

**建议不同 Provider**：避免同源偏差，写者和审者用不同模型家族。

### 架构哲学：拥抱 Bitter Lesson

Missions 系统约 700 行文本（Prompt + Skills），而非复杂硬编码状态机。原因：模型升级时系统自动变强。保留薄层确定性逻辑（Handoff 检查、阻塞条件、里程碑边界）；其余交给模型。

*[Source: raw/Multi-Agent 工程系统.md]*

---

## Claude Managed Agents 原生多 Agent 支持（2026年5月最新）

来源：Anthropic Code with Claude 活动（2026-05-06）

**能力基线**：单任务最多 **20 个专用 Agent 并行**运行（非顺序），Anthropic 托管底层基础设施（通过 Anthropic_Agent_SDK 调用）。

生产验证案例：
- **Netflix**：Fan-Out 模式并行分析数百构建日志
- **Harvey（法律 AI）**：Specialist Team 跨文档协作，Dreaming 启用后完成率提升 **6x**
- **Shopify**：目标 Q3 2026 实现 90% 自主编码

---

## Dreaming（定时自进化机制）

> **核心作用**：Agent 团队跨 Session 积累"机构记忆"，无需人工更新 Prompt。

**机制**：配置 dream schedule（推荐每晚），Agent 在后台回顾历史 Session → 提取成功/失败模式 → 自动更新 Memory Store → 下次 Session 直接携带这些经验。

**实测结果**（Harvey）：启用 Dreaming 后 Agent 完成率提升约 6 倍，与模型版本无关——纯粹来自 Agent 携带跨 Session 学到的机构知识。

**启用方式**：在 Managed Agents API 的 dream schedule 字段配置时间窗口（Memory Store 持久化层详见 [[Managed_Agent_Memory]]）。

**与 [[Agentic_Memory_System]] 的关系**：Dreaming 是 Agentic Memory 中"Episodic Memory → Long-Term Procedural Memory"转化的自动化实现——把单次 Session 的 episodic 记忆提炼为可跨 Session 复用的 procedural 知识。

---

## Outcomes（基于 Rubric 的质量闭环）

> **核心作用**：把"希望 Agent 输出好"升级为"定义好是什么，让 Agent 自我验证"。

**机制**：在任务定义中写明成功 Rubric（评分标准），Claude 完成任务后对照 Rubric 自我评分，若不通过则自动迭代，直到达标再输出。

**示例 Rubric**：
```
- 必须包含 5 家竞品的定价数据；缺少任何一家，completeness < 80% = 不通过
- 分析节必须有 ≥3 个具体洞察（非泛泛而谈）
- 报告字数 < 2000 字
```

**与 Human_In_The_Loop 的关系**：Outcomes 是"自动化质量门控"（LLM-as-judge），HITL 是"人工质量门控"——两者互补。高信心任务用 Outcomes 自动过滤；高风险任务在 Outcomes 通过后再走 HITL。

*[Source: raw/How to Build a Team of AI Agents That Actually Work Together (Full Course).md]*

---

## Shopify 并行 Agent 工程模式（2026年5月）

来源：Shopify Bessemer Venture Partners AI-First Playbook（@zodchiii，2026-05-18）

**基础设施层：LLM Proxy（统一网关）**
```
工程师 → Claude Code / GitHub Copilot / Cursor
              ↓
        LLM Proxy（集中网关）
              ↓
        集中成本控制 + 用量分析 + 模型切换零改动
```
小团队启示：建基础设施层再选工具，才能安全实验多工具。

**Pattern 1：Parallel Agents（并行 Agent）**
```bash
# 终端 1：重构 auth 模块
claude -p "refactor src/auth/ to use the new session handler"

# 终端 2：写集成测试
claude -p "write integration tests for the payment flow"

# 终端 3：更新文档
claude -p "update API documentation for all changed endpoints"
```
工程师角色从"写代码"→"审查 + merge Agent 输出"。

**Pattern 2：Extended Critique Loops（自我辩论循环）**
```
Prompt 模式：
"Propose an architecture for [X].
Then critique your proposal: what breaks at scale?
Then revise based on critique.
Then critique the revision.
Give me final version with confidence levels."
```
适用场景：复杂架构决策（而非所有任务）。

**Shopify 生产力数据**：
- 策略/执行时间比：2024年 30%/70% → 2026年 **70%/30%**
- 生产力提升 ~20%：不是写更多代码，而是测 10 种方案代替 2 种
- 目标：Q3 2026 实现 90% 自主编码

*[Source: raw/The Claude Code Setup Behind Shopify's 23,000 Engineers (Exact Config You Can Copy).md]*

---

## Anthropic 内部 Agent 开发视角（PM 视角）

来源：Alex（Anthropic PM）播客访谈（2026年）

### 自适应思考（Adaptive Thinking）
- 模型根据问题复杂度**自行决定**是否深度思考，不再盲目消耗大量 token
- 简单问题直接回答，复杂推理任务自动触发 extended thinking
- 与 Agentic_Memory_System 中的 Working Memory 概念互补：Adaptive Thinking 决定推理深度，Working Memory 决定上下文范围

### 单向门决策理论（One-Way Door Framework）
Anthropic PM 对决策分类：
- **单向门**（不可逆）：模型架构选择、训练数据方向 → PM 必须高度谨慎，充分讨论
- **双向门**（可逆）：功能实验、产品迭代 → AI 开发成本降低后，**直接实验比分析更快**
- Agent 工程启示：多 Agent 编排决策属于单向门（修改影响全局），单个 Agent 的 Prompt 调优属于双向门

### Anthropic 内部工作文化（可复制实践）
- **书面文化（Writing Culture）**：所有隐性知识必须文档化；会议采用"静默阅读（Silent Read）"后直接在文档中深度讨论（vs 语言表达的发散会议）
- **原型文化（Prototype Culture）**：任何团队（销售/招聘/工程）都主动构建 AI 原型优化自身工作
- **与 [[CLAUDE_md_Best_Practices]] 的关系**：书面文化 = CLAUDE.md 存在的理由（把隐性知识显式化给 AI）

### Agent 角色与性格（Character）设计
- 随着 Agent 进化为自主决策实体，**价值观、信仰、判断力**成为核心设计变量（超越"任务完成率"）
- 高信任场景（如允许 Agent 决策数据库架构）需要"高尚人格"背书，而非仅靠 Guardrails 约束
- 对应 Human_In_The_Loop：信任建立的路径 = 低风险场景积累 → 逐步扩大 Agent 自主权

*[Source: raw/Claude：智胜未来的原生AI工作流.md]*

- [[Multi_Agent_Missions_System]] — Factory Missions 详细实现：Orchestrator/Workers/Validators 三角色 + Validation Contract First
- [[MultiAgent_Concurrent_Write_Research]] — 多 Agent 并发写入冲突（安全分层隔离的核心未解子问题）

---

## Multi-Agent Coordination Patterns (Databricks Production Guide)

来源：Databricks Data & AI Tech Lead Sandipan Bhaumik（2026年）

**Core finding**: Multi-agent failures are distributed systems problems, not AI problems. A 5-agent system has ≥10 potential connection points — each is a failure point (race conditions, state sync issues).

### Choreography vs. Orchestration Decision Matrix

| Pattern | Description | Use When |
|---------|------------|---------|
| **Choreography** | Decentralized, event-driven. Agents publish/subscribe to event bus. | High autonomy, easy to add agents, naturally event-driven |
| **Orchestration** | Centralized coordinator calls agents, manages flow, handles parallel execution | Complex workflows, precise tracing needed, regulated industries (finance) |
| **Hybrid (SAGA)** | Mixed: complex + high autonomy | Financial credit decisions, complex dependent workflows |

**Debugging asymmetry**: Choreography is hard to debug (need strong observability to trace event propagation); Orchestration is debuggable and rollback-friendly. **Most production systems use Orchestration (Databricks recommendation).**

### Immutable State Snapshots (Anti-Race-Condition Pattern)

**Anti-pattern**: Multiple agents read/write the same database record → lost updates, race conditions.

**Pattern**: Append-only versioned state log:
- Each agent receives previous version state
- Processes it and produces new version (never modifying old records)
- Each handoff validates schema + increments version

```python
@dataclass(frozen=True)  # immutable
class AgentState:
    version: int
    data: dict

def handoff(state: AgentState, next_agent) -> AgentState:
    validated = validate_schema(state)
    return next_agent(AgentState(version=validated.version + 1, data=...))
```

**Benefits**: eliminates race conditions, provides complete lineage, binary-search debuggable, rollback-safe.

### Circuit Breaker Pattern for Cascade Failure Prevention

Wrap agent calls in circuit breaker. N consecutive failures → circuit "opens" → fast-fail protects downstream agents. After timeout, circuit "half-opens" to test recovery.

### SAGA Compensation for Distributed Transactions

Each agent implements `execute()` and `compensate()`. On workflow failure, orchestrator calls compensate in reverse order to undo completed steps and return to initial state.

### Production Stack (Databricks)

- **Orchestration**: LangGraph + Mosaic AI Agent Framework (DAG workflows)
- **Agent implementation**: Unity Catalog functions/models (SQL/Python, centralized governance + versioning + discovery)
- **Serving layer**: Databricks Model Serving / AI Gateway (enforces circuit breakers, retries, timeouts, rate limits)
- **Data layer**: Delta Lake (immutable state versions as append-only rows) + MLflow (tracing per agent call: latency, I/O, token usage)
- **Governance**: Unity Catalog (access control, lineage, audit)

*[Source: raw/多代理编排模式详解.md]*

- [[Enterprise_AI_Architecture]] — Enterprise AI architecture context for these patterns
- [[Agent_Governance_Layers]] — Governance framework that Orchestration patterns enforce
- [[SAP_Agent_LangGraph]] — SAP's LangGraph implementation of orchestration

---

## Brain + Swarm 四阶段模式（2026年6月）

来源：@0xRicker — 1个 Opus 4.8 Brain + 300个 Kimi Agent 构建完整 SaaS（2026-06-03）

**核心洞见**：单 Agent 的几何问题（线性执行 4000 步）vs 并行 Agent 的组织结构问题（方向 + 执行分离）。

**原则**：Brain 永远不碰工具。Hands 永远不做判断。

### 四阶段架构

```
1. Decompose（Opus 4.8）
   └── 将一句话目标拆成依赖树 JSON
   └── 标记执行顺序和前置条件
   └── 只规划，不写代码

2. Dispatch（Swarm层，最多 300 个并行）
   └── 每个叶子任务 = 一个 sub-agent
   └── Data Agent / Build Agent / Asset Agent 同时运行

3. Execute（4000 步并行）
   └── 数据层、后端、前端、资产（Landing page + PPT）同时推进
   └── 各轨道在依赖满足的瞬间启动

4. Review（回到 Opus 4.8）
   └── 读取全部输出，对照原始 spec 检查漂移
   └── 这一步是"看起来厉害但实为垃圾"方案的缺失环节
```

### Orchestrator Prompt 设计约束

```python
# 最重要的约束：不要要求 Opus 写应用代码
# Role: you are the orchestrator, not the builder.
# YOUR JOB: decompose into sub-tasks, mark dependencies, emit task tree as JSON
# do NOT write application code yourself.
```

**实测结果**：40 分钟内完成带实时市场数据的 analytics SaaS + 完整销售 Deck，零手写代码。

**与 Kimi K2 的关系**：开源中文模型在并行执行层（Swarm）中表现出色，证明 "Brain + Swarm" 混合的跨平台可行性。

*[Source: raw/I gave Opus 4.8 an army of 300 agents and built a working SaaS in one afternoon.md]*