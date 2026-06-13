---
name: LLM Wiki 降低 95% Token 消耗
description: 用人工可读 Markdown 文件夹替代 RAG 基础设施，Token 使用量降低 95%，因为 LLM 自己维护摘要和索引
type: seed
concept: LLM Wiki vs RAG — 95% Token 节省悖论
hook_insight: "你花了数月搭建向量数据库、Embedding pipeline、语义检索基础设施——Karpathy 用一个 Markdown 文件夹做到了同样的事，Token 消耗少了 95%。RAG 的根本问题是：它在'查询时重建'AI 已经能自己维护的索引"
wiki_link: "[[Karpathy_Methodology]]"
---

# LLM Wiki vs RAG — 95% Token 节省悖论

## 技术核心逻辑

传统 RAG 系统的工作流：
1. 原始文档 → Embedding → 向量数据库
2. 每次查询 → 语义检索 → 拼接原始片段 → 注入 Context

这个流程有一个根本性的浪费：**每次查询都要重建 LLM 已经能自己维护的摘要**。

LLM Wiki 的方法：
1. 原始信息 → LLM 自动生成结构化 Markdown 摘要页
2. LLM 自动维护页面间交叉引用（反向链接）
3. 每次查询直接读取已有摘要，无需重建

**效率来源**：
- RAG 检索 = 每次查询时"重建"摘要；LLM Wiki = "一次生成，永久复用"
- Markdown 文件比向量数据库的基础设施成本低几个量级
- 摄入新信息时，LLM 自动更新 10-15 个相关页面并建立反向链接

## 优缺点对比

| 维度 | LLM Wiki | 传统 RAG |
|------|------|------|
| Token 消耗 | 低（直接读摘要） | 高（每次重建检索） |
| 实时更新 | 较慢（需 LLM 重写页面） | 快（追加 Embedding） |
| 基础设施复杂度 | 极低（文件夹） | 高（向量 DB + Embedding 服务） |
| 可读性 | 高（人工可检查） | 低（向量不可读） |
| 适合场景 | 结构化知识、长期积累 | 海量非结构化文档、实时摄入 |

## 关键洞见

LLM Wiki 不是 RAG 的替代品，而是揭示了 RAG 被过度使用的范围——当知识是结构化的、可以提前组织的，RAG 就是用大炮打蚊子。

[Source: wiki/Karpathy_Methodology.md]
