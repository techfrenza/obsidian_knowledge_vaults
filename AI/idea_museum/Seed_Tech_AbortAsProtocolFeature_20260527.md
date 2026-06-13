---
name: Plan-First Abort As Protocol Feature
description: Plan-Execute工作流中"范围漂移就停止"把中止重新定义为系统设计功能而非失败——停止是最昂贵也是最廉价的操作
type: seed
concept: 中止即协议（Abort as Feature）
hook_insight: "你的AI Agent停下来了——这不是失败，这是它在告诉你Plan阶段遗漏了一个边界条件。能中止的系统比永远运行的系统更可靠，因为前者知道自己的无知边界"
wiki_link: "[[AI_Orchestration_System]]"
---

## 技术核心逻辑

Plan-First 三阶段工作流（[[AI_Orchestration_System]]）：
```
Phase 1（Spec）→ Phase 2（Plan）→ Phase 3（Execution + auto-accept）
规则：范围漂移就停止，返回 Phase 1
```

**传统认知**：Agent 中止 = 失败，需要修复 prompt 让它继续。

**反直觉洞察**：Agent 中止 = Plan 阶段遗漏了一个边界条件被执行阶段暴露。这是正确的系统行为——执行阶段越自主，中止机制越重要。

对比：
- **无中止机制**的 Agent：在错误路径上持续消耗 Token，最终产出错误结果（但看起来完成了任务）
- **有中止机制**的 Agent：在无法满足已批准计划时停止，保留人类判断的入口

## 设计权衡

| 中止策略 | 优势 | 代价 |
|---------|------|------|
| 立即中止（严格） | 最高可靠性，人类控制最大化 | 频繁打断，需更细致的 Plan |
| 重试3次后中止 | 处理短暂偏差（如网络超时） | 可能在错误路径上浪费3次 |
| 永不中止 | 最高自主性，最少人工干预 | 错误静默累积，"完成"≠"正确" |

**核心原则**：Agentic Loop 的成本结构（[[Agentic_Loop]]）决定了中止策略——每次循环都燃烧 Token，中止是最便宜的错误处理。一个设计好的中止比十次错误迭代便宜 100 倍。

**与 Circuit Breaker 的关系**：Plan-First 的"范围漂移中止"是 [[Institutional_Evolution_Flywheel]] 的安全阀——防止飞轮在错误方向上加速。

*[Source: wiki/AI_Orchestration_System.md, wiki/Agentic_Loop.md]*
