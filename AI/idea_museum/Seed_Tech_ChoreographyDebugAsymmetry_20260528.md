---
name: Seed_Tech_ChoreographyDebugAsymmetry_20260528
description: 多Agent系统中，Choreography（分散事件驱动）与Orchestration（集中控制）的调试难度不对称是选择后者的真正理由，而非能力差异
type: seed
concept: 编排模式的调试不对称（Orchestration vs Choreography Debug Asymmetry）
hook_insight: "Choreography 让你的 Agent 系统更灵活——但当它出错时，你需要追踪跨 N 个 Agent 的事件传播链才能定位根因。Databricks 在生产中推荐 Orchestration 不是因为它更强大，而是因为单点追踪让 rollback 只需一个命令；灵活性的代价不是架构复杂度，而是调试时间"
wiki_link: "[[Multi_Agent_Architecture]]"
---

# Choreography vs Orchestration：调试不对称才是决定性因素

## 技术核心逻辑

多 Agent 协调模式两极：

- **Choreography**（编舞）：去中心化，各 Agent 通过事件总线 publish/subscribe，无中央控制器
- **Orchestration**（编排）：中央 Orchestrator 显式调用 Agent、管理流程、处理并行执行

直觉上 Choreography 更"现代"更"弹性"——添加 Agent 只需订阅事件，无需修改中央代码。

## 反直觉权衡

| 维度 | Choreography | Orchestration |
|------|-------------|--------------|
| 扩展性 | 高（添加订阅者零改动） | 中（需更新 Orchestrator） |
| 调试难度 | 极高（需追踪事件传播链） | 低（单点追踪，中央日志） |
| Rollback | 复杂（需撤销分散状态） | 简单（Orchestrator 反序调用 compensate） |
| 生产推荐 | 只限高自治、天然事件驱动 | 大多数生产系统默认选择 |

**不对称点**：5 个 Agent 系统有 ≥10 个潜在连接点，每个都是故障点。Choreography 下故障传播路径数量是 Orchestration 的指数倍。

## 深度洞察

调试不对称在问题规模较小时不可见——3 个 Agent 的 Choreography 调试完全可行。但当 Agent 数量超过 5 个，事件传播追踪难度跨越阈值，成为比架构灵活性更大的工程成本。Databricks 生产数据：大多数多 Agent 系统失败是分布式系统问题，不是 AI 问题。

**Hybrid SAGA 模式**：金融信用决策场景用 SAGA（混合模式），保留 Choreography 的高自治，加入 Orchestration 的 compensate 机制——但这本身说明纯 Choreography 在关键路径下不可靠。

[Source: wiki/Multi_Agent_Architecture]
