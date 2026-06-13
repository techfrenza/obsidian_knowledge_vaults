---
name: Adaptive Thinking Paradigm Inversion
description: Opus 4.7 用「模型自决思考深度」取代「工程师设定 token 预算」——思考量的控制权从人转移到模型
type: seed
concept: 自适应思考主权（Adaptive Thinking Sovereignty）
hook_insight: "你精心设置了 budget_tokens: 10000 来控制 AI 的思考深度——Opus 4.7 把这个参数废掉了，因为让工程师猜测'这道题需要多少思考 token'本身就是错误的问题：只有模型自己知道答案需要多深的推理"
wiki_link: "[[Opus_4_7_Migration]]"
---

# Adaptive Thinking Paradigm Inversion

## 技术核心逻辑

Opus 4.7 的 `adaptive` thinking 是一个认知范式转变，而非 API 升级：

**旧范式（工程师控制）**：
```python
thinking: {"type": "enabled", "budget_tokens": 10000}
# 工程师必须猜：这道题值 10K tokens 的思考？
```

**新范式（模型自决）**：
```python
thinking: {"type": "adaptive"}
# 模型根据任务复杂度动态分配思考深度
```

背后的认识论变化：
- 工程师设定 budget_tokens = 假设工程师了解任务难度
- adaptive = 承认模型比调用者更懂"这道题需要多深的推理"
- effort 参数（high/xhigh/max）只设定上限天花板，不干预分配

## 优缺点对比

优势：
- 消除系统性错误：低估 budget → 推理截断；高估 budget → 浪费成本
- 简单任务节省 Token，复杂任务自动深化推理
- Prompt 工程负担下降（删除"每 3 次 tool call 总结一次"等脚手架指令）

劣势：
- 成本不可预测：同一 prompt 不同运行可能消耗不同 token
- 旧代码必须迁移（`budget_tokens` 字段 → 400 报错）
- 失去对思考过程的精细控制（但这或许本来就是幻觉）

## 关联
- [[Opus_4_7_Migration]] — 完整 API 变更清单
- [[Agent_Harness_Engineering]] — effort 参数与 Harness 厚度关系
- [[Claude_Code_Subagents]] — 4.7 子代理默认减少，需主动要求 fan out

[Source: wiki/Opus_4_7_Migration.md]
