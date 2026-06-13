---
type: seed
source: wiki_scan
date: 2026-05-08
---

# Prompt Template as Cheap Evaluator-Optimiser

**核心逻辑**：多代理 Evaluator-Optimiser 架构（Generator + Evaluator 双 Agent 循环）是目前提升输出质量的黄金方案，但成本是单次调用的 2-5 倍。然而，**一个结构良好的 Prompt 模板已经内嵌了评估者逻辑**。

以 Code Reviewer Prompt #22 为例：
```
检查：安全性、逻辑、性能、可读性、最佳实践。
每个问题：严重程度、位置、原因、修复（展示修改后代码）。
```

这个输出格式要求实际上是在**同一次调用内强制模型执行 Generator + Evaluator 两个角色**——先生成代码判断，再按评估框架输出结构化批评。

**权衡对比**：

| 方式 | 质量 | 成本 | 适用场景 |
|------|------|------|----------|
| 结构化 Prompt 模板 | 中高 | 1x | 单任务、成本敏感、明确评估维度 |
| 双 Agent Evaluator-Optimiser | 高 | 2-5x | 高风险输出、需多轮迭代、评估维度复杂 |

**反直觉点**：你不需要多代理架构就能获得评估-优化的好处。一个好的 Prompt 模板是多代理系统的"穷人版"——在预算受限时应该先穷尽结构化模板，再考虑多代理。

**Wiki Link**: [[Prompt_Template_Library]], [[Harness_Engineering_Deep_Dive]], [[Multi_Agent_Architecture]], [[Prompt_Engineering_Library]]
