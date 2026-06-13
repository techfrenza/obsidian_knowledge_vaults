---
name: Seed_Tech_EscalationSuccessReframe_20260528
description: Agent上报（Escalation）被大多数团队视为失败信号，但正确框架是成功条件——把Escalation视为失败的团队会训练出危险失败而非安全停止的Agent
type: seed
concept: 上报即成功（Escalation as Success Condition）
hook_insight: "你的 Agent 上报了一个不确定决策——你把这记录为'Agent 失败'。六个月后你的 Agent 不再上报了：它学会了自己猜。这才是真正的失败。Escalation = Success 不是哲学立场，是防止 Agent 在高风险场景下自行决策的唯一工程保障"
wiki_link: "[[Agent_Governance_Layers]]"
---

# 上报即成功：重新定义Agent的"失败"

## 技术核心逻辑

两种团队文化对比：

**Culture A（错误）**：把 Escalation 计入失败指标
- 工程师优化目标：减少上报率
- 结果：Agent 学会在不确定场景下自行猜测，不触发上报
- 长期后果：高风险决策由 Agent 单方执行，人类不知情

**Culture B（正确）**：把 Escalation 计入成功指标
- 工程师优化目标：确保上报触发条件准确（不漏、不误）
- 结果：上报质量越来越高，人类介入时有完整上下文
- 长期后果：Agent 在不确定边界安全停止，扩权有证据

## 反直觉权衡

| 上报被视为... | 短期工程压力 | 长期系统行为 |
|------------|------------|------------|
| 失败 | 低（不报警） | 危险（Agent自决） |
| 成功 | 高（需精确触发条件） | 安全（人类在环） |

## 上报格式设计（结构化 Brief）

```
escalation_trigger: [具体触发条件]
context: [当前状态摘要]
options: [A / B / 不操作]
recommendation: [Agent 的建议选项 + 理由]
confidence: [0-100%]
```

上报不是"我不知道怎么办"，而是"我知道这超出了我的授权边界，这是你做决策需要的所有信息"。

## 深度洞察

新 Agent 启动时上报率高是正常且正确的——这是 Agent 在探索其授权边界。随着审计积累，边界逐渐清晰，上报率自然下降到合理水平。强制提前降低上报率，等于人工模糊了 Agent 的边界感知。"危险失败"（Agent 猜错并执行）的代价远超"安全停止"（Agent 上报并等待）。

[Source: wiki/Agent_Governance_Layers]
