---
name: Inversion Pattern - Agent Interviews You First
description: 强制 Agent 在行动前完成所有信息收集，消除"聪明猜测"是 Skill 设计中反直觉的最高可靠性模式
type: seed
concept: Inversion Pattern（Agent 先面试再行动）
hook_insight: 你让 AI 直接帮你规划项目——它给了一个看起来合理的计划，基于它猜测的你的需求。Inversion Pattern 的铁律：AI 不允许输出任何内容，直到它把所有问题都问完
wiki_link: "[[Skill_Design_Patterns]]"
---

## 技术核心逻辑

Skill Design Pattern 4 —— Inversion（反转模式）的核心约束：

```yaml
Instructions:
- DO NOT synthesize final output until ALL phases complete
- Phase 1: Ask structured questions one by one
- Wait for user answer before next phase
- Only after full context, output plan
```

**钻石门（Diamond Gate）**：`DO NOT start building until all phases complete`。这是硬性阻断，不是建议。

## 反直觉洞察

传统 Skill 设计思路：用户输入 → Agent 智能推断缺失信息 → 直接输出。

Inversion Pattern 反转这个逻辑：Agent 主动收集所有缺失变量，完全消除推断环节。**推断越少，可靠性越高**——即使代价是交互轮次增加。

| 对比维度 | 传统模式（直接行动）| Inversion Pattern |
|---------|-------------|-----------------|
| 信息来源 | Agent 猜测 + 用户输入混合 | 100% 用户明确提供 |
| 失败原因 | 猜测错了，用户不知道哪里偏了 | 用户提供错误信息（可追溯）|
| 适用场景 | 简单、标准化任务 | 项目规划、需求收集、高风险决策 |

## 致命陷阱

开发者最常犯的错误：在 Inversion Pattern 的 Skill 里，加入"如果用户没回答，推断默认值"——这彻底破坏了模式的可靠性保证。**宁可卡住，不要猜测。**

[Source: wiki/Skill_Design_Patterns.md]
