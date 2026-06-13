---
name: AI Agent 每 15 次工具调用自动进化——但没人审查它学到了什么
description: Hermes Agent 的自动 Skill 提炼机制是"进化"的，但进化方向不受质量约束——一个关于 AI 自学习系统的 X 平台反直觉 Hook
type: seed
concept: AI 自进化的质量失控（Autonomous Skill Learning Without Quality Gates）
hook_insight: 你的 AI Agent 正在每 15 次工具调用后自动写入新技能——没人审查这些技能是否正确
wiki_link: "[[Hermes_Agent]]"
---

你以为 AI Agent 的进化是"越来越聪明"——

Hermes 的真实情况：

每 15 次工具调用，Agent 自动分析 Session，生成新 Skill 文件，追加到技能库。

没有质量审查。没有测试。没有人工确认。

一次调试混乱的 Session + 15 次工具调用 = 一个新的"技能"被永久写入系统。

更危险的是：这个坏 Skill 开始被调用 → 产生更多混乱 Session → 触发更多坏 Skill 提炼。

这不是 AI 在学习。

这是 AI 在用和你一样高的效率，学习错误的东西。

Hermes 文档自己也说了："需要定期 review 生成的 Skill。"

把质量控制外包给人工 review，然后宣传"全自动进化"。

我们需要自学习系统的 QA 标准，不是更多的自学习系统。
