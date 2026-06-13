---
title: Claude Code Self-Evolving System
aliases: ["自进化系统", "Corrections→Rules 闭环", "Self-Evolving"]
tags: [claude-code, self-evolving, skills, memory, automation]
category: claude-tooling
parent: "[[Claude_Code_Skills]]"
created: 2026-05-15
date: "2026-05-15"
---

# Claude Code Self-Evolving System

Parent: [[Claude_Code_Skills]]

> 通过 Corrections→Rules→Verification 闭环，让 Claude Code 每次 session 后变得更智能。[Source: raw/Claude Code self evolving.md]

---

## 核心架构：4 层认知系统

| 层级 | 组件 | 职责 |
|------|------|------|
| Layer 1 | `CLAUDE.md` | 决策框架 + 质量门控（< 150 行） |
| Layer 2 | Specialized Agents | architect/reviewer，独立 context |
| Layer 3 | Path-scoped Rules | 目录级规则（如 auth 目录才加载安全规则） |
| Layer 4 | Evolution Engine | memory + auto-promote 循环 |

---

## 自进化核心循环

```
Correction 发生
    ↓
自动 log 到 .claude/memory/
    ↓
出现 2 次 → 自动 promote 为 permanent rule
    ↓
生成 grep 验证 check
    ↓
/evolve 每周 review → 毕业 / 修剪规则
```

**目标指标**：每 session correction 次数从初期 3 次降至 0.5 次以下。

---

## 文件夹结构

```
.claude/
├── memory/           ← correction log + evolution state
├── commands/
│   ├── review.md     ← /project:review（pre-commit）
│   ├── fix-issue.md  ← /project:fix-issue
│   └── evolve.md     ← /evolve（weekly review）
├── skills/           ← 可复用 SOP
└── agents/
    ├── architect.md  ← 架构决策专用
    └── reviewer.md   ← 代码审查专用
```

---

## Settings + Hooks 确定性控制

- `settings.json` allow：`npm test`（自由运行）
- `settings.json` deny：`rm -rf`（物理阻断）
- Hooks（Pre/PostToolUse）：自动触发 Lint + Test

---

## Evolution Skill（/evolve 命令）

每周执行：
1. 读取 `.claude/memory/` 中的 correction log
2. 识别出现 ≥ 2 次的模式
3. 将其升级为 `CLAUDE.md` Hard Rule
4. 删除过时或已内化的规则
5. 输出 session scoring 趋势图

---

## 相关链接

- [[CLAUDE_md_Best_Practices]] — Hard Rules 段写法与维护
- [[Claude_Code_Skills]] — Skill 设计与封装
- [[Claude_Code_Hooks]] — PostToolUse 确定性执行
- [[Agent_Harness_Engineering]] — 系统治驭工程整体架构
- [[Unique_Engineering_Insights]] — Skeptical Evaluator 原则：自我评估者天然过度慷慨，自进化循环需引入独立评估者
- [[Generator_Evaluator_Separation]] — 独立 Evaluator 的三种实现方案（CLAUDE.evaluator.md / /clear / worktree）
- [[Context_Engineering]] — 自进化时 correction log 的版本化与遗忘策略（Context Rot 防治）

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图