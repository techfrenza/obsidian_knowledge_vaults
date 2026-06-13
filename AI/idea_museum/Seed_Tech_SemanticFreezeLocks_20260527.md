---
type: seed
tags: [idea-museum, anti-intuition, acquisition, ai-agent, legacy-migration]
created: 2026-05-27
---

Concept: Semantic Freeze Locks（语义冻结锁）

Hook Insight: AI Agent在重构遗留代码时最大的失败模式不是"写错了"，而是"写得太优雅了"——删除了那些看似冗余的"防御补丁"（历史边界条件修复），导致迁移上线后复现多年未见的生产事故。Bending Spoons 的解法：在每个代码库强制植入 `.agents.md` + `memory.md` 控制文件，为特定代码方法打上"语义冻结锁（Lock）"，Agent 在重构前优先解析，标记不可改写区域。

反直觉点：最好的AI代码质量保证机制是"禁止AI优化某些代码"。

Wiki Link: [[Bending_Spoons_Universal_OS]] ↔ [[Multi_Agent_Architecture]]
