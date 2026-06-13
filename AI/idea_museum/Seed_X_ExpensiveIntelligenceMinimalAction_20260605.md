---
name: AI 系统里最贵的工程师是最不该写代码的那个
description: Brain + Swarm 架构揭示：最强最贵的 AI 模型在生产系统中的最优用途是"不执行任何操作"——只做规划和最终审查。花最多钱雇的智能，应该做最少的具体工作。
type: seed
concept: 最贵的智能做最少的操作（Expensive Intelligence, Minimal Action）
hook_insight: 每个月你给 Opus 付高价 Token 费——然后用它来执行文件读写、API 调用、字符串处理。你在用法拉利送外卖。@0xRicker 构建 40 分钟 SaaS 的秘密不是更多 Opus，而是 Opus 只做两件事：1) 把目标拆成 JSON 任务树，2) 最后检查输出是否偏离 spec。所有工具执行全部交给便宜的并行 Agent。这是一个反直觉的人力资源原则迁移到 AI 架构的过程：最贵的人应该做最高密度的判断，不是最多的操作。
wiki_link: "[[Multi_Agent_Architecture]]"
---

## X 平台 Hooks 草稿

**Hook 1（反直觉开头）**：
你给最强的 AI 模型布置了什么任务？如果答案是"读文件、调 API、处理字符串"，你在用法拉利送外卖。

Brain + Swarm 架构的第一原则：Opus 的 Prompt 里明确写着"do NOT write application code yourself."

最贵的模型，最少的手。

**Hook 2（数据驱动）**：
1 个 Opus 4.8 + 300 个 Kimi Agent = 40 分钟完整 SaaS + 销售 Deck，零手写代码。

Opus 在这个系统里做了两件事：把目标拆成 JSON，以及最后 Review 输出。

不是 Opus 变强了。是工程师学会了让 Opus 只做 Opus 该做的事。

**Hook 3（挑战常识）**：
AI 系统设计的最大浪费：让最贵的模型执行最多的操作。

正确答案：最贵的智能做最少的操作，最便宜的并行 Worker 做最多的操作。

这是人力资源原则，不是技术原则。

[Source: raw/I gave Opus 4.8 an army of 300 agents and built a working SaaS in one afternoon.md]
