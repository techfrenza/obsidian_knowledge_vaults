---
name: Closed Loop Quality Gate Paradox
description: 同一模型放入 Closed Loop 后可从 60 分提升到 90 分，Open Loop 的灵活性是输出质量的敌人
type: seed
concept: Closed Loop vs Open Loop 的质量门悖论
hook_insight: 你用 Open Loop 给了 AI 最大自由度——但真正让 AI 从 60 分到 90 分的，是你把它锁进了有 Gate 的 Closed Loop
wiki_link: "[[Loop_Engineering]]"
---

## 技术核心逻辑

| 维度 | Open Loop | Closed Loop |
|------|-----------|-------------|
| 模式 | 探索性自由试错 | 有明确步骤 + 质量门（Gate） |
| 输出质量 | 强大但极耗 Token，易产 Slop | 可靠、可控、可预算 |
| 同一模型分数 | 聊天框 60 分 | 进入好 Loop 后 90 分 |
| 适用场景 | 预算无限、探索新领域 | **大多数生产场景首选** |

## 反直觉洞察

工程师直觉是"给 AI 更多自由 = 更好的结果"。Loop Engineering 的反直觉结论：**约束（Gate）才是质量的来源**，不是自由度。同一模型在有客观 Gate（测试通过、Lint 通过）的 Closed Loop 里，输出质量天花板远高于 Open Loop。

## 优缺点对比

**Closed Loop 优势**
- 有客观质量门（不是"看起来不错"）
- 失败可自动重试到通过
- Token 消耗可预算、可控制

**Open Loop 优势**
- 无边界探索，适合创意类任务
- 不需要预定义"什么算成功"

## 致命错误
"无客观 Gate"是 Loop 工程的头号错误：用"输出看起来不错"替代 Gate，本质上是把 Open Loop 包装成 Closed Loop 的假象。

[Source: wiki/Loop_Engineering.md]
