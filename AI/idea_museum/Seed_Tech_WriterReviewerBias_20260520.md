---
name: Seed_Tech_WriterReviewerBias_20260520
description: 同一 session 写代码的 Claude 在评审时会偏向辩护它自己写的代码
metadata:
  type: project
---

**Concept**: Writer/Reviewer 同源偏见

**Hook Insight**: Anthropic 工程师发现：让 Claude 评审它刚写的代码，成功率远低于另开新 session 评审。原因：模型会"记住"写代码时的设计决策，评审时下意识为这些决策辩护而不是挑战它们。这与人类的 confirmation bias 完全相同——但可以用"新 session = 新无偏见的大脑"来绕过。更激进的做法：writer 和 reviewer 用不同 provider（避免同源偏见），让 subagents 挑战 reviewer 的结论（二次过滤假阳性）。

**Wiki Link**: [[Claude_Code_Advanced_Features]] → [[Claude_Code_Subagents]]
