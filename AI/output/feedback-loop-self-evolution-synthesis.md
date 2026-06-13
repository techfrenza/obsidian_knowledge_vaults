---
date: 2026-06-09
source_notes:
  - "[[Generator_Evaluator_Separation]]"
  - "[[Loop_Engineering]]"
  - "[[Claude_Code_Self_Evolving]]"
  - "[[Agentic_Loop]]"
  - "[[Context_Engineering]]"
  - "[[Claude_Code_Subagents]]"
  - "[[Harness_Engineering_Advanced]]"
  - "[[CLAUDE_md_Best_Practices]]"
  - "[[Multi_Agent_Architecture]]"
  - "[[Agentic_Memory_System]]"
  - "[[Claude_Code_Routines]]"
  - "[[Skill_Engineering_10_Rules]]"
  - "[[Claude_Code_CLI_Reference]]"
tags: [synthesis, feedback-loop, self-evolution, evaluator, harness-engineering, loop-engineering]
---

# Feedback Loop 与自进化架构 — 跨笔记综合

## 综合单元
> 核心笔记：[[Generator_Evaluator_Separation]]、[[Loop_Engineering]]
> 邻居笔记：[[Claude_Code_Self_Evolving]]、[[Agentic_Loop]]、[[Context_Engineering]]、[[Claude_Code_Subagents]]、[[Harness_Engineering_Advanced]]、[[CLAUDE_md_Best_Practices]]、[[Multi_Agent_Architecture]]、[[Agentic_Memory_System]]、[[Claude_Code_Routines]]、[[Skill_Engineering_10_Rules]]、[[Claude_Code_CLI_Reference]]

---

## 一致主线

这一综合单元的所有笔记共享同一核心论断：**模型是整个系统中最不可靠的部件，每一种架构模式都是为了通过确定性结构机制来隔离、约束和验证模型的概率性行为。**

Generator_Evaluator_Separation 要求写作者与批评者绝不共享上下文（作者视角偏差）。Loop_Engineering 坚持质量门（Gate）而非更好的提示词决定输出上限。Context_Engineering 将上下文视为需要主动治理的可变状态。Harness_Engineering_Advanced 将整个范式命名为"系统治驭工程"——将模型视为受控制平面约束的不稳定工程组件。CLAUDE_md_Best_Practices 要求精炼的负向约束规则而非正向能力描述。统一论断：**一切复利机制的前提都是让模型的概率性行为受制于可重复的确定性结构。**

---

## 内在张力

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Open Loop（探索性、不限 token 消耗）是达到 90 分输出的路径；同一模型放进好 Loop 从 60 分升至 90 分 | [[Loop_Engineering]] | 上下文是有限且昂贵的资源；目标是用最少高信号 Token 驱动正确行为；每个 token 都必须"挣钱" | [[Context_Engineering]] |
| Generator 与 Evaluator 必须完全隔离，让 Evaluator 看到生成代码的完整实现上下文会导致作者视角偏差 | [[Generator_Evaluator_Separation]] | Self-Evolving 要求同一 Agent 观察自身 correction 并自动 promote 规则（Correction→Rule 闭环），即由生成者自评 | [[Claude_Code_Self_Evolving]] |
| CLAUDE.md 必须控制在 60-80 行；超过 150 条规则 Claude 会丢规则 | [[CLAUDE_md_Best_Practices]] | Harness Engineering 引入三文件架构（CLAUDE.md + AGENTS.md + DECISIONS.md）加上 `.claude/rules/` 路径级规则，显著增加规则总量 | [[Harness_Engineering_Advanced]] |

---

## 涌现洞察

**自进化循环（Corrections→Rules）本身就是一个反馈循环——而它缺少 Generator-Evaluator 分离所要求的独立 Gate。**

这一洞察只有在同时审视 [[Generator_Evaluator_Separation]] 和 [[Claude_Code_Self_Evolving]] 时才会浮现：前者证明任何让生成者自评的系统都会产生作者视角偏差，后者构建的 `/evolve` 循环正是由模型评估自身 correction 是否值得升级为永久规则。Loop_Engineering 将"有明确质量门（Gate）"定义为 Closed Loop 区别于 Open Loop 的核心特征，而自进化循环没有独立的外部 Gate——它是一个伪装成 Closed Loop 的 Open Loop。没有任何单篇笔记指出这一矛盾；它只在跨笔记视角下才可见。

---

## 知识缺口

**尚未被任何笔记回答的问题**：自进化 Harness 中，规则晋升步骤（rule promotion gate）的最小可行独立评估器应如何设计？

现有笔记描述了什么是 Gate（Loop_Engineering）、为何自评存在偏差（Generator_Evaluator_Separation）、以及规则升级的触发条件（Claude_Code_Self_Evolving：出现 ≥2 次的 correction 自动 promote），但没有任何笔记说明如何在 `/evolve` 命令中引入独立评估者来验证"该 correction 覆盖的是真实历史失败场景"而非"模型认为应该记住的内容"。

**下一步探索建议**：设计一个 `CLAUDE.evaluator.md`，专门作为 `/evolve` 的 promotion Gate——它只读 correction log 和历史失败记录，对每条候选规则打分（是否覆盖真实失败 ≥2 次 / 是否与现有规则重叠 / 是否可 grep 验证），输出 PASS/WARN/FAIL，由人工或独立 Agent 审阅后才允许写入 CLAUDE.md。
