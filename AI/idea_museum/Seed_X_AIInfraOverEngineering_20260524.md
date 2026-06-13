---
name: AI 基础设施最难的部分是你没有搭建的那些
description: LLM Wiki 证明了最复杂的 AI 检索基础设施可以被 Markdown 文件夹替代，真正的工程决策是识别"不需要建"的部分
type: seed
concept: AI 基础设施过度工程化（AI Infrastructure Over-Engineering）
hook_insight: "AI 工程师的进阶标志不是他们搭建了什么复杂系统——而是他们识别并拒绝搭建了什么。Karpathy 用一个 Markdown 文件夹替代了 RAG 基础设施，Token 成本降了 95%。你的向量数据库可能是你最昂贵的技术决策错误"
wiki_link: "[[Karpathy_Methodology]]"
---

# AI 基础设施过度工程化

## Hooks 草稿（1-3 句）

AI 工程的成熟度悖论：越有经验的工程师，往往越倾向于用最简单的工具解决问题。

Karpathy 的 LLM Wiki 证明：你精心搭建的向量数据库 + Embedding pipeline + 语义检索系统，可以被 Claude Code 自己维护的 Markdown 文件夹替代——而且 Token 消耗少 95%。

真正的 AI 工程决策不是"用什么工具"，而是"哪些工具根本不需要"。RAG 基础设施在结构化知识场景中是用大炮打蚊子；LLM 自己就是最好的"自组织摘要引擎"。

[Source: wiki/Karpathy_Methodology.md]
