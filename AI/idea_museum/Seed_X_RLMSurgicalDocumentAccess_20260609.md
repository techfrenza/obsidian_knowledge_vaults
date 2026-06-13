---
name: RLM - Never Feed The Whole Document
description: 把整个长文档塞给 AI 是最普遍的错误——正确做法是让 AI 像外科医生一样用 4 个工具外科式取数
type: seed
concept: RLM 文档外科取数（永远不要一次性塞完整文档）
hook_insight: 你把 5000 行文档复制进 AI——AI 的注意力在第 2000 行之后开始漂移，但你不知道，因为它还在输出
wiki_link: "[[RLM_Simulation]]"
---

## X 平台 Hooks 草稿

**Hook 1（操作反转型）:**
99% 的人用 AI 处理长文档的方式：全部复制粘贴进去，等待答案。

正确方式（RLM）：
- peek()：先看前 2000 字符了解结构
- grep()：过滤相关行
- partition()：平均分成 k 份
- recurse()：每份单独处理

每个 prompt 上下文极短。Context Rot 归零。

你不是在用 AI 处理文档，你是在用 AI 处理文档的一个手术切片。

**Hook 2（成本揭秘型）:**
AI 处理 5000 条客服票据的最贵方式：一次性全部输入。

最便宜且准确率最高的方式：grep 出相关 300 条，再分 10 批处理。

区别不是算力，是信号密度。你给 AI 越多噪声，它花越多注意力在噪声上。

[Source: wiki/RLM_Simulation.md]
