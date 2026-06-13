---
name: 评估者天然过度宽容：生成者与评估者必须分离
description: LLM 评价自己生成的内容时存在结构性过度慷慨偏差，生产系统必须强制将生成角色与评估角色拆分为独立 Agent 实例，并调优评估者保持怀疑态度。
type: seed
concept: Skeptical Evaluator 强制分离（Generator-Evaluator Structural Split）
hook_insight: 你让同一个 Claude 既写代码又审查代码——然后纳闷为什么 Code Review 总是"LGTM"。Anthropic 内部测试发现：模型评价自己的输出时，即使质量平庸也会赞美，这不是 bug，这是 RLHF 训练目标的必然副产品（训练目标让模型倾向于让人满意）。唯一的修复不是换更好的 Prompt，而是把生成者和评估者拆成两个独立实例，并且调优评估者使其保持怀疑态度。Seven-Agent Factory 把这条写成铁律：Validator 发现问题只能写报告，不能动代码。
wiki_link: "[[Unique_Engineering_Insights]]"
---

## 技术核心逻辑

模型评价自身输出的偏差来源于训练结构：

| 角色 | 问题 |
|------|------|
| Generator（生成者） | 生成时优化"让人满意"的输出 |
| Evaluator（同一实例） | 评估时延续相同的"让人满意"偏好 → 倾向于肯定 |
| Skeptical Evaluator（分离实例） | 独立系统指令强制怀疑态度，消除 sunk cost bias |

**正确架构**（Evaluator-Optimizer 模式）：
```
Generator Agent → 生成输出
       ↓
独立 Evaluator Agent（系统指令：保持怀疑，找问题，不要鼓励）
       ↓
修正循环直到评估通过
```

**Seven-Agent Factory 铁律**：
- Validator 只有读权限 + 报告写权限
- Validator 无法修改任何源文件
- 原因：最后一个有写权限的人，恰好是最容易引入回归 bug 的人

## 优缺点对比

**优势**：
- 消除"自我幻觉"——模型不再为自己的输出背书
- Creator-Verifier 模式同时消除 sunk cost bias（审查者对代码没有情感投入）
- 成本可控：Evaluator 角色用更便宜的模型即可（Haiku as Judge）

**劣势**：
- 增加了 token 成本和延迟（至少多一轮调用）
- 需要仔细设计 Evaluator 的评分 Rubric，否则分离架构形同虚设
- 调优 Evaluator 的"怀疑态度"需要防止矫枉过正（过度否定产生有效输出）

## 关联
- [[Unique_Engineering_Insights]] — Skeptical Evaluator 概念来源
- [[Seven_Agent_Software_Factory]] — Validator 只读约束的实现
- [[Multi_Agent_Architecture]] — Creator-Verifier 模式作为五种协作模式之一
- [[SAP_Agent_Evaluation]] — SAP 生产环境的评估框架

[Source: raw/Unique Ideas from NotebookLM.md]
