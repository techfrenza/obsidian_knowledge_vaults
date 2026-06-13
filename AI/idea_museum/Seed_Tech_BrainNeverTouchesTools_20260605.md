---
name: Brain + Swarm 架构：最强模型必须是最无手的
description: 在 Brain + Swarm 多 Agent 架构中，Orchestrator（最强模型）永远不执行工具调用，只负责规划与审查。反直觉：最贵的模型做最少的操作。
type: seed
concept: Brain 永远不碰工具（Brain-Tool Separation Principle）
hook_insight: 你把最强的模型 Opus 放在系统核心——然后让它执行文件读写、API 调用、代码运行。这恰好是最浪费的设计。@0xRicker 用 1 个 Opus 4.8 + 300 个 Kimi Agent 构建完整 SaaS：Opus 只做两件事——将目标拆成依赖树 JSON，以及最终 Review 对照 spec 检查漂移。Opus 的提示词里明确写道："YOUR JOB: decompose into sub-tasks. do NOT write application code yourself." 最强的模型不是手最多的 Agent，而是那个知道自己不该动手的 Agent。
wiki_link: "[[Multi_Agent_Architecture]]"
---

## 技术核心逻辑

Brain + Swarm 四阶段架构中，Opus 4.8（Brain）与 Kimi/Sonnet（Swarm）有严格的职责边界：

| 角色 | 能力 | 禁止操作 |
|------|------|----------|
| Brain（Opus） | 规划、拆解、最终审查 | 直接执行任何工具调用 |
| Swarm（Kimi/Sonnet） | 执行、工具调用、代码生成 | 做方向判断、范围决策 |

关键约束写入 Orchestrator Prompt：
```
Role: you are the orchestrator, not the builder.
YOUR JOB: decompose into sub-tasks, mark dependencies, emit task tree as JSON
do NOT write application code yourself.
```

## 优缺点对比

**优势**：
- Opus 的 token 成本集中在高密度推理（规划），不浪费在低密度操作（文件 I/O）
- Brain 无工具调用意味着其上下文保持纯净，不被工具输出污染
- 实测：40 分钟完成带实时市场数据的 analytics SaaS + 完整销售 Deck，零手写代码

**劣势**：
- 需要严格的 Handoff 协议：Brain 输出的 JSON 依赖树必须被 Swarm 正确解析
- Brain 的 Review 阶段（第 4 步"检查漂移"）是最常被省略的环节，省略后系统变成"看起来厉害但实为垃圾"的方案工厂

## 关联
- [[Multi_Agent_Architecture]] — Brain + Swarm 完整四阶段架构
- [[Harness_Over_Model_Principle]] — 为何最强模型不等于最有效的工具执行者
- [[Agent_Engineer_Mental_Models]] — 角色分离的心智模型

[Source: raw/I gave Opus 4.8 an army of 300 agents and built a working SaaS in one afternoon.md]
