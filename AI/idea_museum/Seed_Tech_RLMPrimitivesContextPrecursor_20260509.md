---
name: RLM Manual Primitives as Context Engineering Precursor
description: peek/grep/partition/recurse 四工具是 Context Engineering 四原语（Write/Select/Compress/Isolate）的手动前身——展示了为什么自动化上下文管理必然出现
type: seed
---

# RLM Manual Primitives — The Manual Predecessor of Automated Context Engineering

[Concept] **你在手动做的事，正是 Harness 需要自动化的事**

| RLM 手动工具 | Context Engineering 原语 | 对应自动化工具 |
|-------------|------------------------|--------------|
| `peek(ctx, n)` | **Select** | RAG top-k 检索 |
| `grep(ctx, pattern)` | **Select** | 向量搜索 + BM25 |
| `partition(ctx, k)` | **Compress** | `/compact`、自动摘要 |
| `recurse(subtask)` | **Isolate** | Subagents、LangGraph 节点 |

[Trade-off]

| 方式 | 优 | 劣 |
|------|----|----|
| 手动 RLM（当前） | 零基础设施，任何 LLM 可用 | 需要用户手动粘贴结果，速度慢 |
| 自动化 Context Engineering | 流畅，可扩展 | 需要 Harness 基础设施 |

关键洞见：RLM Simulation 是一个**思想实验的可操作版本**——它暴露了 Context 管理的本质是四个动词（看/选/压/隔），让工程师理解 Harness 在"替你做什么"。

[Wiki Link] [[RLM_Simulation]] · [[Context_Engineering]] · [[Agent_Harness_Engineering]]
