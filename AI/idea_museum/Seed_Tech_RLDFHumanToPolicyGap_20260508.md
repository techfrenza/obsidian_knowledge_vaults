---
type: seed
source: wiki_scan
date: 2026-05-08
---

# RLDF: The Human-to-Policy Conversion Problem

**核心逻辑**：Harness Engineering 的开放问题之一是"RLDF 优化"——如何将人工 PR Review 转化为全组织可机械执行的"黄金原则"。这是一个**非对称速度问题**：

- 人类产出洞见的速度：每次 PR Review ≈ 1 个隐性规则
- 机械执行规则的速度：Hooks + CLAUDE.md 可以瞬间应用到所有 Agent

**权衡对比**：

| 路径 | 转化效率 | 瓶颈 |
|------|----------|------|
| 人工总结 PR Review → 手写 CLAUDE.md 规则 | 低（依赖工程师记得） | 人类记忆衰减 |
| 自动化"Correction Compounding"闭环 | 高（每次失败自动补规则） | 规则质量参差不齐 |
| Evaluator Agent 扫描 PR → 生成 AGENTS.md 条目 | 中（需要良好 Evaluator 设计） | Evaluator 的"怀疑论"校准 |

**反直觉点**：工程师花大量时间写 PR 评论，这些评论里包含着最高质量的"黄金原则"——但几乎没有系统将它们自动转化为可执行约束。RLDF 是把人类的直觉变成机器合规保证的关键缺失环节。

**Wiki Link**: [[Harness_Engineering_Deep_Dive]], [[AI_Team_Coding_Practice]], [[Claude_Code_Self_Evolving]], [[CLAUDE_md_Best_Practices]]
