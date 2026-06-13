---
name: Seed_Tech_CompiledTruthVsAppendOnly
description: GBrain 对同一知识库使用两种相互矛盾的写入策略，通过分层解决一致性与完整性的天然对立
type: seed
concept: Compiled Truth vs Append-only Duality
hook_insight: 同一个知识页面，顶部内容是可变的（最新理解覆盖旧理解），底部内容是不可变的（时间线只追加不修改）——这不是设计矛盾，而是刻意的分层认识论
wiki_link: "[[GBrain_Architecture]]"
---

## 技术核心逻辑

GBrain 的每个实体页面（人/公司/会议/书）分为三层，写入策略截然不同：

```
┌─────────────────────────────────────┐
│ Compiled Truth（顶部）               │ ← 可变：当前最佳理解，新信息覆盖旧理解
├─────────────────────────────────────┤
│ Append-only Timeline（时间倒序）      │ ← 不可变：历史事件只追加，永不修改
├─────────────────────────────────────┤
│ Raw Data Sidecar（原始来源）          │ ← 只读：transcript/PDF，证据链
└─────────────────────────────────────┘
```

**为什么不能只用一种策略？**

- **只用 Append-only**：AI 每次需要回答"此人当前的立场是什么"时，必须遍历整个历史时间线。随着知识库增长，推理成本线性增加。
- **只用 Compiled Truth**：失去审计能力——无法回答"这个结论是何时、基于什么证据形成的"，也无法溯源检查编译逻辑是否有误。

## 权衡对比

| 策略层 | 读取效率 | 写入风险 | 审计能力 | 适用场景 |
|--------|---------|---------|---------|---------|
| Compiled Truth | 极高（O(1) 直接读取） | 中（覆盖旧理解可能引入错误） | 无直接审计 | AI 实时推理时的工作记忆 |
| Append-only Timeline | 低（需遍历） | 无（只追加） | 完整 | 溯源、争议仲裁、训练数据 |
| Raw Data Sidecar | 极低 | 无 | 最完整 | 验证 Compiled Truth 准确性 |

## 工程启示

这个三层结构与数据库的 CQRS（命令查询职责分离）模式同构：
- Compiled Truth = Read Model（针对查询优化）
- Append-only Timeline = Event Log（针对审计和重建优化）
- Raw Data Sidecar = Source of Truth（最终裁判）

参见：[[Agentic_Memory_System]] 的 External Store + Episodic Log 分层；[[AI_Team_Coding_Practice]] 的 DECISIONS.md 追加策略。
