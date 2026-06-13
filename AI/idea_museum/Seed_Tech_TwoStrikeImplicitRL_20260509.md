---
name: Two-Strike Rule — Implicit RL via Correction Promotion
description: 自进化系统中的"两次即规则"机制：correction 出现 2 次自动 promote 为 permanent rule，是无需标注的隐式强化学习
type: seed
---

# Two-Strike Rule — Implicit RL via Correction Promotion

[Concept] **让 Claude Code 自我学习的最小可行强化信号**

```
Correction 发生一次 → 记录到 .claude/memory/
Correction 出现 2 次 → 自动 promote 为 CLAUDE.md Hard Rule
Hard Rule → 生成 grep 验证 check
每周 /evolve → 毕业 / 修剪过时规则
```

[Trade-off]

| 设计选项 | 优 | 劣 |
|----------|----|----|
| 阈值 = 1 次 | 响应快 | 噪音大，偶发错误会成为永久规则 |
| **阈值 = 2 次（当前）** | 过滤偶发，保留系统性问题 | 需更多对话触发 |
| 人工 review 所有 correction | 质量高 | 失去自动化价值 |

关键洞见：这是**隐式 RLHF**——不需要奖励模型，不需要标注员，correction log 本身就是反馈信号。两次出现 = 足够的统计置信度，值得固化为规则。

[Wiki Link] [[Claude_Code_Self_Evolving]] · [[CLAUDE_md_Best_Practices]] · [[Unique_Engineering_Insights]]
