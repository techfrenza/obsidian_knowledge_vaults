---
concept: LLM 错误率从 41% 到 11%
hook: 四条规则能把 Claude 的代码错误率砍掉 73%——但大多数人的 CLAUDE.md 超过 200 行，全部白费
wiki_link: [[Karpathy_Methodology]]
tags: [seed, anti-intuitive, karpathy, claude-md]
---

**Concept**: Karpathy 4 Rules = AI 行为治理的最小有效剂量

**Hook Insight**: Karpathy 发现 CLAUDE.md 超过 200 行时 Claude 遵守度急剧下降，重要规则被噪声淹没。这意味着大多数人精心维护的长篇 CLAUDE.md 可能是反效果的——越多规则，越没有规则。真正有效的行为治理是"最小有效剂量"：4 条规则，错误率从 41% → 11%。

**X Hook Draft**: 我测试了 Karpathy 4 Rules 的实际效果：让 AI 先陈述假设（不猜）、最少代码（不过设计）、手术式修改（不重构未损坏的代码）、给成功标准（不给步骤）。结果：错误率从 41% 降到 11%。但有个反直觉细节：你的 CLAUDE.md 超过 200 行了吗？超过的话，这 4 条规则也会失效。
