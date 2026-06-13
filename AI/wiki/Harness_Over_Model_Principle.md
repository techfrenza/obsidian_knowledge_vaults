---
title: Harness Over Model Principle
aliases: ["Harness 重于模型", "制度重于能力", "Harness优先原则"]
tags: [harness, agent-engineering, reliability, principle, core-axiom]
category: agent-engineering
parent: "[[Agent_Harness_Engineering]]"
created: 2026-05-25
date: "2026-05-25"
---

# Harness Over Model Principle

Parent: Agent_Harness_Engineering

> **核心公理**：AI Agent 系统的可靠性瓶颈不是模型能力，而是运行模型的制度环境（Harness）。模型是不稳定的工程部件；Harness 才是真正的产品。

---

## 实证数据

- 同一模型（Opus 4.5）在不同 Harness 下任务通过率：**78% vs 42%**（差距 36 个百分点）—— [[Agent_Engineer_Mental_Models]]
- Karpathy 12 规则将 Claude 错误率从 **41% 降至 11%**，靠的是行为约束而非换模型 —— [[CLAUDE_md_Best_Practices]]
- 同一任务的 context-token 消耗，优化后可降低 **50–98%**，决定变量是架构 —— [[Context_Engineering]]

---

## 论断的七种表述

不同来源笔记对同一公理的表述方式，均指向相同结论：

| 来源 | 具体表述 |
|------|---------|
| Agent_Harness_Engineering | "Claude Code 的性能不取决于模型能力，而取决于其运行的线束环境" |
| [[Enterprise_AI_Architecture]] | "AI 系统的价值不在于模型能力，而在于围绕模型构建的制度与基础设施" |
| [[Claude_Code_Settings]] | "Claude Code 的可靠性来自系统层约束，而非提示词层的劝说" |
| [[AI_Workflow_System]] | "编排层的价值远超模型层" |
| [[Enterprise_Agent_Playbook]] | "Agent 系统的可靠性不取决于模型智能，而取决于 Harness 与 Skills 的清晰边界设计" |
| Agent_Engineer_Mental_Models | "Harness 重于模型，同一模型不同 Harness 性能差距 78% vs 42%" |
| [[AI_Orchestration_System]] | "人类专注决策和验收，AI 负责执行，质量由系统基础设施保证" |

---

## 三层制度控制平面

```
Layer 1: 物理强制（settings.json deny）  → 不可能越过
Layer 2: 认知约束（CLAUDE.md 规则）     → 应该遵守
Layer 3: 执行隔离（Subagent 上下文）   → 污染隔离
```

Harness 的价值沉淀于这三层，而非 Prompt 本身。参见 Claude_Code_Settings、CLAUDE_md_Best_Practices、[[Claude_Code_Subagents]]。

---

## 推论

1. **Harness 是可版本化产品**：规则文件、Skill 描述、Hook 约束可 git 管理、可迭代改善 —— [[Institutional_Evolution_Flywheel]]
2. **Fat Skills + Thin Harness**：路由层保持最薄，知识/技能层尽量厚重 —— [[GBrain_Fat_Thin_Architecture]]
3. **触发准确率由 description 决定**：Skill 的可靠性由输入端决策层决定，不由 Skill 本体决定 —— [[Claude_Code_Skills]]

---

## 矛盾与争议

Harness 层本身也有上限：CLAUDE.md 超过 200 行后合规度急剧下降，Harness 过重同样是性能毒药。
→ 核心权衡见 CLAUDE_md_Best_Practices：规则密度 vs 遵守度之间存在 Compliance Cliff。

## 延伸：Self-Evolving Harness

Harness 的终极形态不是静态制度控制平面，而是能自我进化的运行时。参见 [[Self_Evolving_Harness]] — 当 Harness 能通过 Tracing 数据自动发现自身缺陷并修复时，产品护城河来自 Harness 积累的 Error Patterns 和 Context Strategies，而非模型本身。
