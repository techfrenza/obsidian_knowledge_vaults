---
title: Agent Context Architecture
aliases: ["AI Agent 上下文架构", "四层记忆体系"]
tags: [agent, context, memory, episodic, semantic, procedural]
category: memory-systems
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Agent Context Architecture

Parent: [[index]]

> 核心论点：企业 Agent 的决策质量由上下文架构决定。"懂规则"≠"懂此刻该怎么做"——只有分层结构化的记忆系统才能让 Agent 在复杂业务中稳定行动。

---

## 四层 Context 分层架构

| 层级 | 文件夹 | 内容 | 检索方式 |
|------|--------|------|----------|
| 情境记忆 (Episodic) | `context/episodic/` | `[日期][参与人][事件] → [决策/输出]` | `brain ask [关键词] --type episodic` |
| 语义记忆 (Semantic) | `context/semantic/` | 规则/术语/流程/共识 | Agent 默认先查，再决定行动 |
| 程序化记忆 (Procedural) | `.claude/skills/` `SOP/` | SOP + Skill 文件（frontmatter + 步骤 + 输出格式 + out-of-scope） | 触发词自动加载 |
| 工作记忆 (Working) | `context/working/[task-id]/` | 当前任务专用上下文 | 任务启动时 Assemble |

---

## 递归蒸馏与回注闭环

每周运行一次（5 分钟启动）：

```
读 context/episodic/ 本周所有记录
→ 向上抽象：生成周报级模式/趋势/判断
→ 更新 semantic/ 和 procedural/
→ 向下回注：生成下周记录模板（带结构重点）
```

**遗忘策略**：每月 review，运行 `forget low-frequency items older than 30 days`，低用内容降权或移档。

---

## Context Assembly 任务驱动组装

启动新任务命令：
```
Assemble context for [具体目标]：
从 semantic 取规则、procedural 取 SOP、episodic 取最近3条相关事件
输出结构化 working 包，限制 <4000 tokens
```

---

## Context Reframing 卡点解法

```
用 Context Reframing 重构当前问题：
调整边界/目标/视角，把现有 episodic 记录重新组织
输出3条新行动路径
```

---

## 立即行动清单

1. 项目根目录建 `Context.md` + 四个子文件夹，按模板初始化
2. 挑 1 个正在跑的 Agent 任务，运行一次 Context Assembly
3. 晚上让 Agent 做本周递归蒸馏
4. 高频重复任务立即打包 1 个 Procedural Skill
5. 每周固定时间跑 forget & decay，保持系统轻量

---

## 关联实体

- [[Agentic_Memory_System]] — 四类内存的技术实现（In-context / External / Episodic / Parametric）
- [[Agent_Harness_Engineering]] — 将 Context 架构嵌入 Harness 的整体框架
- [[Cross_Platform_Memory]] — 跨 AI 平台使用 Markdown 文件迁移记忆
- [[AI_Team_Coding_Practice]] — 团队侧 AGENTS.md/DECISIONS.md 上下文资产体系（Context Engineering 实践）
- [[Enterprise_AI_Architecture]] — 企业级 Context Engineering 与 File-system-as-State
- [[RLM_Simulation]] — 手动模拟 RLM 处理超长上下文的 peek/grep/partition/recurse 四工具（Context Rot 防治的操作层实现）
- [[Context_Engineering]] — Context is State 的完整工程化实现（四大原语 + Context Rot 对策）
- [[Agent_Engineer_Mental_Models]] — 上下文原语作为第三大心智模型的理论层
- [[index]] — 主索引

*[Source: raw/AI Agent Context.md]*
