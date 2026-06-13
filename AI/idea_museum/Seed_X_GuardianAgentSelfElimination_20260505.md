---
type: seed
source: wiki_scan
date: 2026-05-05
---

# Seed: Guardian Agent 的自我消灭目标

**[Concept]** 好的安全 Agent 设计目标是让自己越来越少被触发——HITL 拦截率下降才是成功指标，不是拦截率上升。

**[Hook]**
> "大多数人认为 AI 安全系统的成功指标是：拦截了多少危险操作。
> 实际上，如果你的 Guardian Agent 每天拦截 50 次，说明你的 Agent 设计失败了。
> 好的安全架构是：Agent 逐渐学会边界，拦截趋向于零。
> Guardian Agent 的终极成功是让自己失业。"

**[反直觉核心]** HITL 不是监控层，是**纠错教师**。每次拦截都应该被写回为 Golden Dataset 中的新约束。随着约束积累，Agent 的越界行为减少，拦截次数下降。拦截率持续高说明约束没有被写回——系统在重复犯同样的错误。这与 [[Claude_Code_Self_Evolving]] 的 Correction→Rule 闭环是同一逻辑在安全层的投影。

**[Wiki Link]** [[Human_In_The_Loop]] → [[Enterprise_AI_Architecture]] → [[Claude_Code_Self_Evolving]]

*[Source: wiki/Human_In_The_Loop.md, wiki/Enterprise_AI_Architecture.md | 2026-05-05]*
