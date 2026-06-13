---
type: seed
source: wiki_scan
date: 2026-05-06
---

# Seed: 四类记忆的不对称使用陷阱

**[Concept]** Agentic Memory 四分区中，Parametric（模型权重）是唯一"免费"的，却是最危险的

**核心技术逻辑**

[[Agentic_Memory_System]] 定义四类内存：

| 类型 | 成本 | 准确性风险 |
|------|------|-----------|
| In-context | Token 消耗 | 溢出即遗忘 |
| External | 存储 + 检索延迟 | 向量漂移 |
| Episodic | Write + Embed 成本 | 检索精度依赖嵌入质量 |
| **Parametric** | **零成本** | **时间衰减 + 幻觉** |

**权衡核心**：开发者倾向于"让模型自己知道"（依赖 Parametric），因为无需编写任何代码。但 Parametric 记忆有截断日期，且对私有/敏感数据**完全无效**。

**反直觉结论**：越"省力"的记忆层越不可靠。真正生产级 Agent 应该把 Parametric 降级为 fallback——wiki 明确写"时间敏感/私密内容绝不依赖"，但大多数 demo 从不提这条。

**Why it matters**：企业级 Agent 最常见的幻觉来源不是 Prompt 写错，而是把公司内部政策、近期数据、客户信息托付给了 Parametric 层。

**[Wiki Link]** [[Agentic_Memory_System]] — 四类内存分区 + 完整 Memory Flow
