---
name: SAGA Compensation Makes Every Agent Reversible
description: 为每个 Agent 实现 compensate() 方法是分布式 AI 事务的反转设计——不防失败，而是让失败可逆
type: seed
concept: SAGA 补偿机制（Agent 级可逆事务设计）
hook_insight: 你在思考如何防止 Agent 失败——Databricks 生产实践的答案是：别防，让每个 Agent 实现 compensate()，失败了就倒带
wiki_link: "[[Multi_Agent_Architecture]]"
---

## 技术核心逻辑

SAGA 模式在 Multi-Agent 架构中的实现：

```python
class Agent:
    def execute(self, state) -> AgentState:
        # 执行正向操作
        ...
    
    def compensate(self, state) -> AgentState:
        # 撤销本 Agent 的所有操作
        ...

# 失败时由 Orchestrator 逆序调用 compensate()
# Agent N 失败 → compensate N → compensate N-1 → ... → compensate 1
```

## 权衡分析

| 设计理念 | 防失败（加守卫）| SAGA 补偿（设计可逆）|
|---------|-------------|-----------------|
| 复杂度分布 | 前置检查层 | 每个 Agent 实现 compensate |
| 失败处理 | 尽量不失败 | 失败了自动回滚 |
| 适用场景 | 简单单一操作 | **涉及多 Agent 的分布式事务**|
| 已知最佳实践 | 单体系统 | 金融、合规等高风险场景 |

## 深层洞察

SAGA 补偿的反直觉之处：**它不试图消除失败，而是把失败从"破坏性事件"变成"可控回滚操作"**。

在 Multi-Agent 系统中，这一设计哲学比传何错误预防更重要：当 5 个 Agent 并行操作时，同时防御所有可能的失败组合在工程上不可行；但给每个 Agent 写一个 compensate() 却是线性工作量。

**实际应用**：与 Immutable State Snapshot 配合——不可变状态 + 可补偿操作 = 分布式 Agent 事务的完整解决方案。

[Source: wiki/Multi_Agent_Architecture.md]
