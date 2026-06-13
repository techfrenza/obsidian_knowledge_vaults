---
type: seed
source: wiki_scan
date: 2026-05-08
---

# Plan-and-Execute vs ReAct：速度与适应性的根本权衡

**[Concept]** Plan-and-execute 架构将任务规划与执行完全分离：Planner LLM 先生成完整执行图，再并行分发给 Executor agents。ReAct 架构则是每步观察后即时决策（Observe → Think → Act 循环）。

**[Trade-off Core]**
- **速度**：Plan-and-execute 达到 3.6x 加速（LLMCompiler benchmark），因为执行步骤可并行化
- **适应性**：ReAct 每步都能根据环境反馈调整策略；Plan-and-execute 一旦计划生成，中途无法响应意外输出
- **失败模式**：Plan-and-execute 的致命弱点是"计划污染"——初始规划错误会传播到所有并行执行节点；ReAct 的错误是局部的

**[Non-obvious Insight]** 3.6x 加速的代价不是"灵活性稍差"，而是**错误放大系数**。一个错误在 ReAct 中影响 1 步，在 Plan-and-execute 中可能同时污染 4-6 个并行分支。适用条件：任务分解确定性高时用 Plan-execute，探索性任务用 ReAct。

**[Wiki Link]** → [[Agent_Harness_Engineering]]（七个架构决策 #2）→ [[LangGraph_Build_Agents]]（Evaluator-Optimizer 模式）→ [[Agentic_Loop]]（四阶段成本分析）
