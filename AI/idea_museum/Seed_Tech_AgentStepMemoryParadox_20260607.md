---
name: SAP Agent Step 计量的内存节约悖论
description: SAP Agent Step 计量将"Memory Lookups"定义为零成本步骤，但这反而激励 Agent 依赖记忆而非重新计算——而记忆失效时，付费的修复步骤会增多
type: seed
concept: Agent Step 计量的内存激励悖论（Agent Step Metering Memory Incentive Paradox）
hook_insight: SAP 把 Memory 查询定为"免费步骤"，结果激励工程师把越来越多的逻辑塞进记忆——当记忆过期，你会为修复支付更多 Steps
wiki_link: "[[SAP_Agent_Ship_Checklist]]"
---

## 技术核心逻辑

SAP Agent Step 计量规则（TR13）：

| 操作类型 | 计为 Agent Step | 计费 |
|----------|----------------|------|
| LLM call + OData 读写 | ✅ 1+ Steps | 是 |
| Memory Service lookups | ❌ 0 Steps | 否 |
| 技术错误/重试 | ❌ 0 Steps | 否 |

**被忽略的第二阶效应**：

```
Memory 节约设计 → 更多逻辑依赖 Memory Cache
→ Memory 过期/失效（不可避免）
→ Agent 基于过期记忆做出错误决策
→ 纠错需要额外 LLM + OData 步骤（付费）
→ 实际 Step 消耗 > 直接计算的基线
```

对比：[[Tokenmaxxing]] 中提到"Agent Step（业务价值单元）vs Token（算力消耗）是企业 AI 定价范式迁移的早期信号"——但 Step 的计量边界本身就包含激励扭曲。

## 优缺点对比

**零成本 Memory Lookup 设计意图**：
- 鼓励使用持久记忆，避免重复昂贵计算
- 降低简单查询类 Agent 的成本

**潜在代价**：
- 工程团队的优化目标变成"最大化 Memory Hit Rate"，而非"最小化总错误"
- Memory 一致性问题的修复成本被转移到"计费 Steps"里，在账单上不可见

## 关联

- [[SAP_Agent_Ship_Checklist]] — TR13 计量规则
- [[SAP_Agent_Memory_Service]] — Memory Service 的具体实现
- [[Tokenmaxxing]] — Token vs Step 的定价范式对比
- [[Agent_Governance_Layers]] — 计量边界的治理含义
