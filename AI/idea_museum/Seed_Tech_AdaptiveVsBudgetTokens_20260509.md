---
name: Adaptive Thinking vs Budget Tokens — Declarative vs Imperative Effort
description: Opus 4.7 用"adaptive"替代"budget_tokens"：将思维深度从数字预算变为声明式意图，是 LLM API 设计从命令式到声明式的缩影
type: seed
---

# Adaptive Thinking — Declarative vs Imperative Effort

[Concept] **从"给多少 Token 想"到"你自己判断该想多深"**

```python
# Opus 4.6（命令式）— 告诉模型分配多少算力
thinking: {"type": "enabled", "budget_tokens": 10000}

# Opus 4.7（声明式）— 告诉模型意图，让它自决
thinking: {"type": "adaptive"}
effort: "xhigh"
```

[Trade-off]

| 方式 | 优 | 劣 |
|------|----|----|
| `budget_tokens`（命令式） | 精确控制成本上限 | 工程师不知道"10000 够不够"，需反复调参 |
| `adaptive`（声明式） | 模型自适应任务难度，避免过思或欠思 | 成本不可精确预测 |

关键洞见：这是 API 设计的哲学转变——**从"给资源"到"声明目标"**。类比：不告诉 SQL 引擎用哪个索引，只声明查询意图。LLM 的控制层也在向更高抽象层迁移。

[Wiki Link] [[Opus_4_7_Migration]] · [[Context_Engineering]] · [[Agent_Engineer_Roadmap]]
