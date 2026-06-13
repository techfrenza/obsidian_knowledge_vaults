---
type: seed
source: wiki_scan
date: 2026-05-05
---

# Seed: 上下文隔离是并行的代价，不是设计失误

**[Concept]** 多 Agent 并行执行时，每个 Agent 的"无知"（隔离 Context）恰恰是保证质量的必要条件，不是 bug。

**[Hook]**
> "你开了 5 个并行 Agent session，它们互相不知道彼此在做什么。
> 你以为这是架构缺陷，急着建共享内存让它们'互相感知'。
> 这是最常见的多 Agent 设计错误。
> 隔离不是缺陷，是防止幻觉传染的防火墙。"

**[反直觉核心]** 长上下文引发的"逻辑幻觉"（Context Rot）会在多 Agent 之间传播——一个 Agent 的错误推理污染共享上下文，导致其他 Agent 基于错误前提继续执行。企业架构的选择是：Orchestrator-Subagent 模式中，子代理必须在**隔离 Context** 中执行，通过结构化输出（不是共享上下文）传递结果。并行的代价是协调成本，但这个代价比幻觉传染的代价低一个数量级。跨领域类比：微服务不共享数据库的原因一样——局部故障隔离 > 实时一致性。

**[Wiki Link]** [[Enterprise_AI_Architecture]] → [[Claude_Code_Subagents]] → [[RLM_Simulation]]

*[Source: wiki/Enterprise_AI_Architecture.md | 2026-05-05]*
