---
title: Agent Engineering Primitives
parent: "[[Agent_Engineer_MOC]]"
tags: [agent-engineering, primitives, evals, frameworks, 2026]
category: agent-engineering
stub: false
date: "2026-06-03"
---

# Agent Engineering Primitives

2026年 Agent 工程的持久原语（Durable Primitives）——这些技能和模式在框架迭代中复利增长，而不是每季度重学。

## 5-Test 框架：过滤新框架/工具发布

在采纳任何新 Framework/Tool 前运行5项测试：

| 测试 | 问题 | 通过标准 |
|------|------|---------|
| 1. 2年后重要性 | 是包装器（wrapper）还是原语（primitive）？ | 原语（协议/内存模式/沙箱方案）→ 是；包装器 → 否 |
| 2. 生产后证 | 有没有受尊重的团队写过 postmortem？ | 有真实 postmortem，而非营销博文 |
| 3. 迁移成本 | 采用是否要求丢弃现有 tracing/retries/config/auth？ | 是 → 框架试图成为平台（90%死亡率），跳过 |
| 4. 跳过成本 | 跳过6个月的成本是什么？ | 大多数为零。等6个月看清楚再决定 |
| 5. 可度量性 | 能否衡量它对你的 Agent 是否有帮助？ | 没有 Evals = 靠直觉跑回归 |

**核心习惯**：新工具发布时，写下"6个月后要看到什么才信它重要"。90天后回来检查。

## 持久原语（Durable Primitives）

### 1. Context Engineering

最重要的重命名：Prompt Engineering → Context Engineering。

> "模型的行为是你放入 context window 的内容的涌现属性。"

- Context is State：每个 token 的不相关噪音都在消耗推理质量
- **Context Rot**：到 10 步任务的第8步，原始目标可能已被工具输出掩埋
- 修复方法：主动总结/压缩/剪枝；版本化工具描述；缓存静态部分；拒绝缓存会变化的部分

**实战检验**：打开生产 agent 的完整 trace，对比第1步和第7步的 context。数一数有多少 token 还在"挣钱"。

### 2. Tool Design

工具是 Agent 与业务的交汇点。模型基于**名称和描述**选择工具，基于**错误消息**决定重试。

**5-10个命名清晰的工具 > 20个平庸工具**：
- 工具名应读作英语动词短语
- 描述应包含：何时使用 + 何时**不**使用
- 错误消息应为模型可行动的反馈：
  - 差：`"Error: 400 Bad Request"`
  - 好：`"Max tokens 500 exceeded, try summarizing first"`

**数据**：一个团队仅重写错误消息，retry loop 减少 40%。工具调优的 ROI 远高于 prompt 调优。

### 3. Orchestrator-Subagent 模式

**2024-2025 多 Agent 争论的结论**：

- 并行写入共享状态的多 Agent = 灾难（错误复合）
- 单 Agent loop 扩展性远超预期
- **唯一可生产的多 Agent 形态**：Orchestrator 将窄范围只读任务委托给隔离 Subagents，然后综合结果

**核心约束**：Subagents 只读（不修改共享状态）；Orchestrator 拥有写操作。

**何时添加**：默认单 Agent → 当 context window 成为瓶颈/顺序工具调用的延迟/真正异构任务 → 才引入 Orchestrator-Subagent。

### 4. Evals + Golden Datasets

> "每个发布可靠 Agent 的团队都有 Evals。每个没有的团队，没有。"

**构建方式**：
1. 从生产 traces 中收集失败样本
2. 标注失败 → 形成回归集
3. 新失败发生 → 立即追加到集合
4. LLM-as-judge 处理主观部分；精确匹配/程序化检查处理其余
5. 任何 Prompt/模型/工具变更前先跑 suite

**数据**：Spotify 的 judge 层在发布前否决约 25% 的 agent 输出。没有 judge，每4个不良结果就有1个到达用户。

**关键心智模型**：Eval = 在一切变化时让 agent 保持诚实的单元测试。模型升级/框架变更/端点废弃时，Eval 是唯一知道 agent 是否仍在工作的东西。

### 5. File-System-as-State + Think-Act-Observe

所有真实多步骤 Agent 的持久架构：Think → Act → Observe → Repeat，以文件系统或结构化存储为事实来源。

**设计逻辑**：模型是无状态的；Harness 必须有状态。文件系统是每个开发者都已理解的有状态原语。

**核心洞察**：Harness 做的工作比模型更多——验证动作、沙箱运行、捕获输出、决定回馈内容、决定何时停止/检查点/派生子 Agent。换一个同等质量的模型，好的 Harness 仍能运行；换一个差的 Harness，最好的模型也会随机"忘记"它在做什么。

## 框架选型（2026年4月）

### 推荐使用

| 框架 | 适用场景 |
|------|---------|
| **LangGraph** | 生产默认（约1/3大型公司）；类型化状态/条件边/持久工作流/HITL检查点 |
| **Mastra** | TypeScript 生态首选 |
| **Pydantic AI** | Python + 类型安全优先（2025年底 v1.0） |
| **Claude Agent SDK** | Computer Use/Voice/Real-time（不做顶层 Orchestrator） |
| **MCP** | 所有工具集成；标准化 tool registry；"AI的USB-C" |
| **LangGraph + E2B/Browserbase** | 沙箱代码执行（必须沙箱，无例外） |

### 记忆框架选型
- **Mem0**：chat 式个性化，用户偏好/轻度历史
- **Zep**：生产对话系统，状态演化，实体追踪
- **Letta**：跨日/周/月的 Agent 连贯性
- **默认**：先用 context window + 向量存储；感受到失败模式后再升级

### 可观测性
- **Langfuse**：OSS 默认（MIT license，tracing + prompt versioning + LLM-as-judge evals）
- **LangSmith**：LangChain 生态首选
- **Braintrust**：研究风格 eval，严格对比
- 必须同时有 Tracing（发生了什么）和 Evals（比昨天更好还是更差）

## 明确跳过的框架

| 框架 | 原因 |
|------|------|
| AutoGen/AG2 | 交由社区维护，发布停滞，抽象不匹配生产需求 |
| CrewAI | 易 demo 难生产；实际构建者已转移 |
| Semantic Kernel | 除非锁定 MS 企业栈 |
| 并行共享状态的多 Agent | Demo 好看，生产崩溃 |
| "autonomous agent" 独立部署 | 2023的思路；2026正确框架 = "agentic engineering"（监督/有界/可评估） |
| 每周追新模型 | 按季度评估。中间时间用于构建 |

## 生产采纳路径（无聊但有效）

1. **选一个具体可测量的结果**（不是"agent 平台"）
2. **先建 Tracing + Evals**（发布前，不是之后）
3. **从单 Agent loop 开始**（LangGraph + 3-7个精心设计的工具 + 文件系统作为状态）
4. **把 Agent 当产品而非项目**（失败 trace = 路线图）
5. **只在失败模式拉入时增加范围**（subagents / memory / computer use）
6. **从第一天监控单位经济**（每次操作成本、缓存命中率、retry 循环成本）
7. **按季度而非每周重评估模型**

## 关联知识
- [[Agent_Engineer_Learning_Path]] 6阶段学习路径
- [[Harness_Engineering_Deep_Dive]] Harness 构建原则
- [[Harness_Over_Model_Principle]] Harness 重于模型的核心公理（实证数据）
- [[Context_Engineering]] 上下文工程
- [[Production_Agent_Engineering]] 生产 Agent 四大原语
- [[MCP_Production_Decision_Framework]] MCP 生产决策框架
- [[Agentic_Memory_System]] 记忆框架对比
- [[Multi_Agent_Architecture]] Orchestrator-Subagent 模式
- [[Prompt_Injection]] 沙箱安全：提示注入攻击防御
- [[Self_Evolving_Harness]] Harness 自进化：Tracing 为核心

[Source: raw/What to Learn, Build, and Skip in AI Agents.md]
[Source: raw/Grok and Gemini Chats.md]
