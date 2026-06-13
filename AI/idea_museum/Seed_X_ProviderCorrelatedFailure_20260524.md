---
name: 你花钱买冗余却买到了相关故障
description: 同一云提供商的多个模型同时失败的概率远高于预期，真正的冗余需要跨提供商多样性
type: seed
concept: 提供商级相关故障（Provider-Level Correlated Failure）
hook_insight: "你在同一个云上部署了 Claude Sonnet、Claude Haiku、Claude Opus 作为备份——恭喜，你花了三倍的钱，但可靠性几乎没有提升。当 Anthropic 的 API 出问题时，这三个备份会同时失败。真正的冗余不是同提供商冗余，是跨提供商多样性"
wiki_link: "[[SAP_Agent_Resilience]]"
---

# 提供商级相关故障（Provider-Level Correlated Failure）

## Hooks 草稿（1-3 句）

工程师的本能冗余策略：在同一云平台上部署多个模型版本作为 fallback。

这个策略的致命缺陷：同一提供商的所有模型共享相同的基础设施、网络层和认证系统。SAP 生产系统中记录的观察：**一个 Anthropic 模型在 SAP AI Core 上失败，通常意味着所有 Anthropic 模型同时失败**。

真正的弹性不是 `Claude Sonnet → Claude Haiku → Claude Opus`，而是 `Anthropic Sonnet → Azure GPT-4o → Google Gemini`。冗余的粒度必须在提供商级，不是模型级。

[Source: wiki/SAP_Agent_Resilience.md]
