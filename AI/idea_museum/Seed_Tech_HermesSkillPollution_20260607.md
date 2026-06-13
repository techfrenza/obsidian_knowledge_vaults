---
name: Hermes 自学习技能污染悖论
description: Hermes Agent 每 15 次工具调用自动写入 Skill，但低质量 Session 也会触发——导致技能库被噪声污染，且污染是静默发生的
type: seed
concept: 固定触发阈值 vs 质量过滤的自学习代价（Hermes Skill Auto-Write Paradox）
hook_insight: 你的 Agent 在学习——但它不区分好课和坏课，只数到 15 就开始"总结经验"
wiki_link: "[[Hermes_Agent]]"
---

## 技术核心逻辑

Hermes Agent 的自进化机制：每 15 次工具调用后，自动分析本次 Session，生成 `.skill` 文件追加到技能库。触发条件是**固定次数阈值**，而非输出质量判断。

| 机制 | 设计意图 | 潜在缺陷 |
|------|----------|---------|
| 15-call trigger | 低摩擦、持续积累 | 无质量门控，低质量 Session 同等触发 |
| 自动写入 skill 文件 | 减少手动维护 | 噪声 Skill 静默进入路由匹配池 |
| 无人工 Review 步骤 | 全自动化 | 错误可复利：坏 Skill 被调用 → 产生更多坏 Session → 触发更多坏 Skill |

## 优缺点对比

**优势**：
- 零摩擦知识积累，真正的"越用越聪明"
- 不需要工程师手动归纳 Pattern

**缺陷**：
- 污染是静默的——你不知道哪个 Skill 来自一次"调试乱跑"的 Session
- 自学习系统的质量上限取决于 Session 质量的平均水平，不是最佳水平
- [[Hermes_Agent]] 自己提到需要"定期 review"生成的 Skill——等于把质量控制外包给了人

## 关联

- [[Hermes_Agent]] — 自进化机制来源
- [[Skill_Engineering_10_Rules]] — 对比：规则 5 要求 LLM-as-Judge 评估才算完整；Hermes 跳过了这一步
- [[Claude_Code_Self_Evolving]] — Claude Code 侧的 Corrections→Rules 闭环；对比两者的质量控制机制
