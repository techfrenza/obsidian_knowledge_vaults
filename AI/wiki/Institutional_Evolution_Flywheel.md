---
title: Institutional Evolution Flywheel
aliases: ["制度演化飞轮", "错误资本化", "Self-Reinforcing Loop", "Skillify 飞轮", "制度飞轮"]
tags: [flywheel, self-evolving, institutional-design, reliability, core-pattern]
category: enterprise-ai
parent: "[[Agent_Harness_Engineering]]"
created: 2026-05-25
date: "2026-05-25"
---

# Institutional Evolution Flywheel（制度演化飞轮）

Parent: Agent_Harness_Engineering

> **核心模式**：每次 Agent 失败不是损失，而是为下一次运行积累确定性知识。错误→规则更新→约束增强→错误率下降→新错误暴露→下一轮，形成自强化闭环。

---

## 规范术语说明

此模式在知识库中出现过四个不同命名，均指向同一机制：

| 曾用名 | 来源 | 统一为 |
|--------|------|--------|
| 制度演化飞轮 | [[Enterprise_AI_Architecture]] | **Institutional Evolution Flywheel** |
| 错误资本化 | `enterprise-gbrain-agent-architecture-synthesis.md` | 同上 |
| Self-Reinforcing Loop | [[Enterprise_Agent_Playbook]] | 同上 |
| Skillify 飞轮 | [[GBrain_Architecture]] | 同上 |

**选定理由**：Institutional Evolution Flywheel 最准确描述了机制本质——"制度"（而非技术）层面的持续演化，且 Flywheel 比 Loop 更强调加速复利效应。中文规范名：**制度演化飞轮**。

---

## 飞轮结构

```
错误发生
  ↓
立即记录（CLAUDE.md / DECISIONS.md / Skill Gotchas）
  ↓
规则更新（新约束 / Skill 描述修正 / Hook 添加）
  ↓
约束增强（下次运行时生效）
  ↓
该类错误率下降
  ↓
新类型错误暴露（更高层级问题浮现）
  ↓
回到顶部
```

飞轮的转速取决于：**错误被捕获到规则更新的延迟**。延迟越短，飞轮转速越高。

---

## 四层实现（个人→企业）

此模式在不同规模下各有实现，本质相同：

| 层次 | 实现方式 | 来源 |
|------|---------|------|
| 个人 CLI | Karpathy Loop：Claude 犯错后立即更新 CLAUDE.md | [[CLAUDE_md_Best_Practices]] |
| 个人 Brain | GBrain Skillify：重复任务完成后自动生成 SKILL.md | GBrain_Architecture |
| 企业 Playbook | N8N 收集 Session Logs → Claude 自动优化 Prompt + 更新规则 | Enterprise_Agent_Playbook |
| 工程 Harness | Harness Engineering Advanced：每周审查精简规则，删除过时约束 | [[Harness_Engineering_Advanced]] |

---

## 内在张力

飞轮的核心矛盾：

**速度飞轮** vs **质量闸门**
- GBrain/Enterprise Playbook：先运转飞轮，快速迭代，自动 Skillify
- Skill Engineering 10 Rules：每个 Skill 需经过 10 步闭环，未通过全部步骤 = 不是 Skill

**规则增殖** vs **CLAUDE.md 上限**
- Self-Evolving 循环会持续产出新规则
- CLAUDE.md 超过 200 行后合规度急剧下降 → CLAUDE_md_Best_Practices

→ 解法：飞轮必须有**剪枝机制**（每周审查 + 删除过时规则），否则飞轮会被自己的重量拖慢。

---

## 相关笔记
- [[Harness_Over_Model_Principle]]：飞轮是 Harness 超越模型能力的核心机制
- CLAUDE_md_Best_Practices：规则密度上限约束
- GBrain_Architecture：个人层面的 Skillify 实现
- Enterprise_Agent_Playbook：企业层面的 Self-Reinforcing Loop
- [[Claude_Code_Self_Evolving]]：CLI 层面的自进化闭环
- [[Agent_Payments_Risk_Matrix]]：飞轮规则的一个具体落地：支付风险矩阵持久化入 CLAUDE.md
