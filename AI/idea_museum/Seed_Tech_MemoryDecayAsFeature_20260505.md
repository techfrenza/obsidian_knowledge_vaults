---
type: seed
source: wiki_scan
date: 2026-05-05
---

# Seed: 遗忘即特性 — 记忆系统的主动衰减设计

**[Concept]** 高质量 Agent 记忆系统的核心不是"记更多"，而是"精准忘记"——遗忘策略与存储策略同等重要。

**[Hook Insight]** 直觉：Agent 记忆越多越好。现实：无衰减的记忆系统会逐渐充满低价值噪声，导致检索质量下降（向量相似度被旧、低质记忆稀释）。解决方案是主动遗忘：写入时 LLM 打分（只存 >7 分项）+ 时间衰减（recency × semantic_relevance）+ 夜间合并（相似记忆合并为单条 canonical summary）。这是反直觉的权衡：**一个有遗忘机制的 500 条记忆系统，比没有遗忘的 5000 条系统检索效果更好**。记忆系统的质量由出口（遗忘/衰减）决定，不只由入口（存储）决定。

**[Tech Trade-off]**
- 无限积累：覆盖全面、无遗漏 → 缺点：检索精度随时间下降，维护成本线性增长
- 主动衰减：高信噪比、检索稳定 → 缺点：可能丢失低频但关键记忆，需校准阈值

**[Wiki Link]** [[Agentic_Memory_System]] → [[Agent_Context_Architecture]]

*[Source: wiki/Agentic_Memory_System.md | 2026-05-05]*
