---
name: DeterministicLogicLeakage
description: Karpathy Rule 5：将重试/路由/升级等确定性逻辑放在 LLM 内部会产生随机性失败；越聪明的模型越不适合做确定性决策
type: seed
concept: 确定性逻辑泄漏入 LLM
hook_insight: "你雇了世界上最聪明的员工，然后让他决定'API 返回 429 时等几秒重试'——他可能每次给你不同的答案。越智能的模型，越不应该做确定性判断；Rule 5 的本质是：语言模型做概率工作，代码做确定性工作，边界不能混"
wiki_link: "[[CLAUDE_md_Best_Practices]]"
---

## 技术核心逻辑

Karpathy Rule 5 — Don't Make the Model Do Non-Language Work：
- **确定性决策**（是否重试、如何路由、何时升级）= 写在 Harness 代码中
- **概率性决策**（如何理解需求、怎么生成内容、哪个方案更好）= 交给模型

**为什么非直觉**：工程师直觉上认为"更强的模型能更好地做决策"——但对于确定性决策，"更好"没有意义，"一致"才有意义。模型在每次调用时会根据上下文微调，这本身就是非确定性的来源。

## 失败模式

| 确定性逻辑放在 LLM 内 | 确定性逻辑放在 Harness 代码 |
|----------------------|---------------------------|
| 重试策略随上下文变化 | 重试策略 100% 一致 |
| 路由结果不可预测 | 路由可单元测试 |
| 升级条件模糊 | 升级条件明确可审计 |

**架构原则**：Rule 5 是 Harness Engineering 的语言版本——Thin Harness 的"thin"指"薄但确定"，所有随机性都隔离在语言模型层，所有确定性都隔离在代码层。

**关联**：→ [[Harness_Engineering_Deep_Dive]]（Thin Harness 设计哲学）

*[Source: wiki/CLAUDE_md_Best_Practices.md]*
