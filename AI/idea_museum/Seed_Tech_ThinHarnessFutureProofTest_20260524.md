---
name: Harness 未来验证测试
description: 判断 Harness 设计正确性的标准不是当前有多强，而是更强模型出现时是否无需增加复杂度
type: seed
concept: Thin Harness 未来验证测试（Future-Proof Test）
hook_insight: "工程师总在问'这个 Harness 设计够不够强？'——Anthropic 的答案是：这是个错误的问题。正确的问题是：当更强的模型出现时，你需要给 Harness 加复杂度才能利用它吗？如果需要加，你的设计从一开始就错了"
wiki_link: "[[Harness_Engineering_Deep_Dive]]"
---

# Thin Harness 未来验证测试

## 技术核心逻辑

Harness 厚度存在两种极端选择：

- **Thick Harness（厚）**：把大量逻辑硬编码到 Harness 层——复杂重试策略、精细路由规则、大量补偿性 Prompt 工程。短期性能好，但模型升级后 Harness 逻辑可能与新模型行为冲突。
- **Thin Harness（薄）**：Harness 只负责路由、工具注入、物理阻断。模型升级后，无需改 Harness，性能自动提升。

Anthropic 的赌注是 Thin Harness：模型越来越强，Harness 应该随之变得更简单而不是更复杂。

**未来验证测试**（Future-Proof Test）：
> "如果更强的模型出现，你的 Harness 不需要加复杂度就能利用它的能力提升——这才是设计正确的证明"

## 优缺点对比

| 维度 | Thin Harness | Thick Harness |
|------|------|------|
| 模型升级适应性 | 高：改模型即可，Harness 不变 | 低：需要大量重新调整 |
| 短期性能 | 较低：依赖模型能力 | 高：可用复杂逻辑补偿 |
| 维护成本 | 低 | 高：模型行为变化时可能产生冲突 |
| 适用时机 | 模型快速迭代期（2024-2026+） | 模型稳定期 |

## 洞见

Thick Harness 本质上是在用工程复杂度**补偿**模型能力不足，而不是**增强**它。这意味着它的价值会随模型进步**递减**，而 Thin Harness 的价值会随模型进步**递增**。

[Source: wiki/Harness_Engineering_Deep_Dive.md]
