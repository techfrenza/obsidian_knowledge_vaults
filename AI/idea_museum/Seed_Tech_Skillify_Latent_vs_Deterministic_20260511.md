---
name: Seed_Tech_Skillify_Latent_vs_Deterministic_20260511
description: The fundamental insight that agent reliability comes from routing work to the right computational space
type: reference
---

**Concept**: Latent vs Deterministic Space — 技能可靠性的本质

**Hook Insight**: Agent 犯错的根本原因不是"不够聪明"，而是**在错误的计算空间执行任务**——日历查询（确定性）用 LLM 推理，时区换算（确定性）用心算。Skillify 的真正价值不是"保存提示词"，而是将确定性任务从 LLM 推理空间永久迁移到脚本执行空间，使错误在结构上不可能重现。

**Why Counter-Intuitive**: 人们以为 Agent 智能化 = 更多 LLM 推理。实际上，好的 Agent 设计是**减少** LLM 推理，把确定性工作交给确定性代码，LLM 只负责判断"何时做什么"。

Wiki Link: [[GBrain_Architecture]]
