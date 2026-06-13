---
name: 测试规格应在代码前写完——不是为了 TDD，而是为了分布式 AI 协调
description: Validation Contract First 的真正理由不是测试驱动开发，而是让 20 个并行 Agent 对准同一个"完成"定义
type: seed
concept: 契约先于代码的分布式协调价值（Contract-First as Distributed Coordination）
hook_insight: TDD 告诉你先写测试——大多数工程师不听，因为在写代码前很难写出好测试。Multi-Agent 系统给了你一个全新的理由：不是为了测试质量，而是因为如果 20 个并行 Agent 没有共同的"完成"定义，它们会彼此冲突。Validation Contract 不是测试文档，它是分布式锁
wiki_link: "[[Multi_Agent_Architecture]]"
---

# 测试规格应在代码前写完——不是为了 TDD，而是为了分布式 AI 协调

## Hooks 草稿

Hook 1（重新框架）：
你听过 TDD，先写测试。你不做，因为在代码存在前写好测试很难。Factory Missions 给了你一个全新动机：不是为了测试质量，而是因为 20 个 AI Agent 并行工作，没有共同"完成"定义，它们会产生 20 种不兼容的实现。Validation Contract 是分布式锁，不是测试文档。

Hook 2（规模压力）：
单个工程师可以靠 slack 沟通对齐"什么叫 Done"。20 个并行 AI Agent 没有 slack。写完代码再补测试在单工程师场景可行，在多 Agent 场景是架构错误——协调协议必须先于执行存在。

[Source: wiki/Multi_Agent_Architecture]
