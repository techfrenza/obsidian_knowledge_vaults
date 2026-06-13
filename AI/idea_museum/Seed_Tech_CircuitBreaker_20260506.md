---
concept: Circuit Breaker for AI Agents
hook_insight: 熔断（circuit breaker）是 LLM 系统的必要组件，但几乎没人默认实现它。连续 autocompact 失败 ≥3 次就停止——不是因为问题无解，而是因为继续循环只会烧 API 预算，不会产出任何结果。这和电路中的保险丝逻辑完全一致：宁可断路，不可过载。
wiki_link: "[[Agent_Harness_Engineering]]"
---

# Seed: AI Agent 熔断机制

**[Concept]** Circuit Breaker 熔断机制应用于 LLM Agent 循环

**[Hook Insight]**  
大多数 AI 工程教程教你如何让 Agent 重试，但没有人教你什么时候**停止重试**。  
连续 autocompact 失败时继续循环 = 往一个破水桶里倒水。  
生产级系统的隐性约束：**失败次数上限（≥3 次停止）是和 retry 逻辑同等重要的工程决策**。

**[Wiki Link]** [[Agent_Harness_Engineering]] — 容错恢复层级章节
