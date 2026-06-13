---
name: 企业正在按"智能步数"给 AI 计费——不是按 Token
description: SAP Agent 的 TR13 计量单位是"Agent Step"（一次 Agentic 循环 = 一步），而非 Token 消耗——这是企业 AI 定价范式悄悄发生的根本性迁移
type: seed
concept: 企业 AI 从 Token 定价迁移到 Agent Step 定价（Agent Step Pricing Paradigm Shift）
hook_insight: 你还在讨论 Token 成本——大企业已经在按"AI 做了多少个决策动作"收费了
wiki_link: "[[SAP_Agent_Ship_Checklist]]"
---

你知道 SAP 怎么给 AI Agent 定价吗？

不是 Token。

不是 API 调用次数。

是 **Agent Step**。

一次 Agentic 循环（LLM 推理 + 工具调用 + 结果处理）= 一个 Agent Step。

定价分三档：
- Basic：5 AI Units/Step（简单工具调用）
- Standard：10 AI Units/Step
- Advanced：25 AI Units/Step（复杂推理）

这背后的逻辑：你不是在为"AI 想了多少"付费，你是在为"AI 帮你做了多少个有价值的业务操作"付费。

这是企业 AI 从基础设施层（算力）向业务价值层（结果）的定价迁移。

当定价单位从 Token 变成 Step——

架构优化目标也跟着变：从"节省 Token"变成"减少无效循环步骤"。

最有趣的细节：Memory 查询是 0 Steps（免费），技术错误重试也是 0 Steps（免费）。

你开始明白为什么 AI 架构的每一个设计选择都在隐性地影响账单了。
