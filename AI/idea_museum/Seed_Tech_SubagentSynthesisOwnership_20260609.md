---
name: Subagent Synthesis Ownership
description: 主 Agent 必须亲自理解子 Agent 结果，禁止二次外包理解工作，否则信息链断裂但系统不报错
type: seed
concept: Sub-agent 综合所有权（禁止二次外包理解）
hook_insight: 你让 Orchestrator 把子 Agent 的结果直接转交给下一个 Agent——你刚刚创造了一个没人真正理解任何内容的流水线，但它看起来完全正常
wiki_link: "[[Harness_Engineering_Advanced]]"
---

## 技术核心逻辑

Harness Engineering 进阶原则中的 Sub-agent 分区原则第二条：

> **强制合成**：主 Agent 必须亲自理解子 Agent 结果后写后续指令，禁止二次外包理解工作。

这条规则的底层逻辑：每次 Agent 传递信息而不处理，信息丢失率累积。5 层转发链 = 5 次无损压缩的假象。

## 权衡分析

| 设计 | 效率 | 可靠性 | 失效模式 |
|------|------|--------|---------|
| Orchestrator 直接转发子 Agent 输出 | 高（零处理延迟）| 低 | 误解静默传播，错误在末端才暴露 |
| Orchestrator 亲自理解后再指令 | 中（增加一次 LLM 调用）| 高 | 误解在综合点立即可见 |

## 深层洞察

"禁止二次外包理解工作"不是对 Agent 能力的不信任，而是对**信息熵增定律**的工程响应：每次无损传递都是信息压缩，压缩必然有损。Orchestrator 亲自理解 = 在每个 Handoff 点做信息熵检查。

**实际表现**：二次外包的流水线在正常案例下表现完美，在边缘案例下静默失败——这是最危险的失效模式，因为系统没有任何报错信号。

[Source: wiki/Harness_Engineering_Advanced.md]
