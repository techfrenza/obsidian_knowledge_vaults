---
name: Opus 4.7 Adaptive Thinking 控制权转移
description: budget_tokens 被废除意味着工程师无法再手动控制 AI 的"思考深度"——控制权从人转移到模型自身，这是一个关于"谁最了解任务难度"的哲学性架构决策
type: seed
concept: 思考深度控制权从工程师转移到模型（Thinking Budget Sovereignty Inversion）
hook_insight: 你精心设计的 budget_tokens 不是被升级——它被删除了，因为 Anthropic 认为你根本没资格决定这道题需要多少思考
wiki_link: "[[Opus_4_7_Migration]]"
---

## 技术核心逻辑

变更对比：

| 参数 | Opus 4.5 及以前 | Opus 4.7 |
|------|----------------|---------|
| `budget_tokens` | 工程师设置思考上限（如 10000 tokens）| **废除**，发送则返回 400 错误 |
| `adaptive` thinking | 不存在 | 替代方案，由模型自行决定思考深度 |
| `effort` 级别 | high / max | high / xhigh / max（新增 xhigh）|

**权力转移本质**：
```
过去：工程师 → 猜测任务需要多少 token → 设置 budget_tokens
现在：模型 → 感知任务复杂度 → 自动分配思考资源
```

这不是功能升级，这是**架构哲学声明**：人类对"这道题需要多少思考"的判断比模型的判断更差。

引发的连锁反应：
- 所有依赖 `budget_tokens` 的 Harness 代码**必须**重写
- 工程师对 Token 成本的预测能力下降（adaptive = 不确定性上升）
- 但 `effort` 参数保留了高层次控制（xhigh 适合编码/代理任务）

对比 [[Adaptive_Thinking_Sovereignty]]（如存在）和 [[Tokenmaxxing]] 中的 Token 成本控制原则。

## 优缺点对比

**Adaptive Thinking 优势**：
- 模型对每道题自行校准，避免简单题"过度思考"浪费 Token
- 工程师从"猜测"解放，减少错误配置

**被转移的代价**：
- Token 成本不可预测（对 CFO 不友好）
- 调试困难：无法知道模型"选择了多少 Token 思考"
- 旧 Harness 的迁移成本（所有 budget_tokens 硬编码全部需要删除）

## 关联

- [[Opus_4_7_Migration]] — 迁移指南和 API 变更
- [[Tokenmaxxing]] — Token 成本控制与预测
- [[Agent_Harness_Engineering]] — effort 参数与 Harness 厚度
