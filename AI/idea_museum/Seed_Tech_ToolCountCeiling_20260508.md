---
type: seed
source: wiki_scan
date: 2026-05-08
---

# 工具数量上限 ≤10：认知负荷天花板，而非工程惯例

**[Concept]** 每个 Agent 的可用工具数量应限制在 10 个以内。这不是任意的工程约定，而是源于 LLM 注意力机制的实证限制：当工具数量超过阈值，模型在语义相似的工具之间的选择错误率显著上升。

**[Trade-off Core]**
- **≤10 工具**：模型可以可靠区分工具语义，正确选择调用目标
- **>10 工具**：工具描述之间的语义重叠触发模型混淆，选错工具概率非线性增长
- **解决方案的代价**：通过 MCP Hub Project 或 Skills 封装"工具包"可突破此限制，但引入了间接层，增加调试复杂度

**[Non-obvious Insight]** 限制不来自模型容量，而来自**上下文中的语义区分度**。同一模型，给它 15 个工具与给它 8 个工具+1 个工具路由器，后者准确率更高——原因是工具路由器把语义消歧工作前移，避免在 Tool Select 阶段同时处理 15 个候选的相互干扰。工具设计的核心约束是"描述互斥性"，而非数量本身。

**[Wiki Link]** → [[Agent_Harness_Engineering]]（架构决策 #6）→ [[MCP_Production_Decision_Framework]]（Context-Efficient Client 模式）→ [[Claude_Code_Skills]]（Skill 六大模式）
