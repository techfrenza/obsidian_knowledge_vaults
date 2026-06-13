---
name: 你的 AI 安全防御有几层"是同一层"
description: 反直觉观点：多层 AI 安全架构中，依赖同一 LLM 的层不是独立层，实际独立防御层数远少于宣称层数
type: seed
concept: AI 安全层数虚报（Security Layer Inflation）
hook_insight: 你建了 6 层 AI 安全——但其中 2 层依赖同一个 LLM 遵守规则。对抗性 Prompt 一次绕过 2 层。你实际上只有 4 层独立防御。"N 层防御"的数字没有意义，除非你报告"其中独立层数 M"
wiki_link: "[[SAP_Agent_Guardrails]]"
---

# 你的 AI 安全防御有几层"是同一层"

## Hooks 草稿

Hook 1（数字反转）：
你以为你有 6 层 AI 安全防御。SAP 工程文档揭示：第 3 层和第 4 层依赖同一个 LLM。对抗性 Prompt 一次绕过两层。你实际有 4 层独立防御——不是 6 层。"N 层安全"是营销，独立层数才是工程事实。

Hook 2（普遍化）：
每次你听到"我们有多层 AI 安全保护"，问一个问题：这些层里，有几层的判断逻辑不依赖 LLM？那个数字才是你真正的防御深度。

Hook 3（行动建议）：
构建 AI 写操作 Agent 的安全架构时，先数"无 LLM 硬防御层"，再数"基于 LLM 软防御层"。把关键业务约束（金额上限、权限检查）放进硬防御层——不是提示词，是代码。

[Source: wiki/SAP_Agent_Guardrails]
