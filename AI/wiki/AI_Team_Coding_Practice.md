---
title: AI Team Coding Practice
aliases: ["团队 AI 编码实践", "AGENTS.md", "DECISIONS.md", "复利编码循环"]
tags: [team-coding, agents-md, decisions-md, compound, context-assets, verification]
category: agent-engineering
parent: "[[index]]"
created: 2026-05-01
date: "2026-05-01"
---

# AI Team Coding Practice

Parent: [[index]]

> 核心论点：团队使用 AI 的复利来自**机器可读[[Agent_Context_Architecture|上下文资产]]积累**，而非单次代码生成速度。每次任务结束后的 Compound 步骤（写回 DECISIONS.md）是长期价值的来源。

---

## 三个核心上下文资产文件

| 文件 | 内容 | 更新时机 |
|------|------|----------|
| `AGENTS.md` | 构建命令、测试命令、约定、避坑清单 | Agent 第二次犯错后立即更新 |
| `DECISIONS.md` | 架构选择、被拒绝方案、已知失败模式 | 每次任务结束 Compound 步骤 |
| `CLAUDE.md` | Hard Rules + 团队标准 | 规则变更时 |

**快速触发**：Agent 犯错后立即输入：
> "把这个约束写进 AGENTS.md 并更新 CLAUDE.md。"

此后所有 session 自动继承，避免重复解释背景。

> 关联：[[CLAUDE_md_Best_Practices]] — CLAUDE.md 60-80 行写法规范

---

## Plan → Work → Review → Compound 四步闭环

```
Plan      → 输出任务定义 + 验收标准（人工确认）
Work      → Agent 执行
Review    → 跑确定性验证（自动通过才算完成）
Compound  → 把方案选择、拒绝理由、bug 模式写回 DECISIONS.md
```

**80% 价值在 Plan 和 Compound**，Work 和 Review 可交给 Agent。

每日结束 Prompt（直接复制）：
> "Summarize decisions and lessons from this session, output ready to append to DECISIONS.md."

---

## 确定性验证基础设施

任何任务前先定义验收标准（复制模板）：
```
写完后必须通过：
vitest 全绿 + tsc --noEmit + lint +
手动 golden case 对比 + 部署前 staging 验证
```

**工具链**：
- [[Claude_Code_Settings|pre-commit hook]]（自动执行）
- CI 自动跑测试 / lint / security scan

验证越确定 → Agent 越能自我迭代 → Review 队列缩短。

> 关联：[[Claude_Code_Hooks]] — postEdit hook 自动执行 lint/format

---

## AGENTS.md 规则写法（显式 + 多示例）

```markdown
Never touch payment module without owner approval.

Example of bad change:
  直接修改 payment/processor.ts 的 charge() 函数

Correct pattern:
  1. 在 #payment-team Slack 频道发出变更 RFC
  2. 等待 owner 审批后才动手
```

规则必须：显式（不含模糊词）+ 附反例 + 附正确做法。

---

## 关键指标切换

| 旧指标（避免）| 新指标（推荐）|
|-------------|-------------|
| 代码行数 | 代码存活率（3 个月内未被改动）|
| PR 数量 | 变更失败率（Change Failure Rate）|
| 完成速度 | Review 时间（Time to Review）|

资深工程师角色从逐行 review → **意图评估 + 风险判断**。

---

## 负面复利避坑清单

| 风险 | 对策 |
|------|------|
| 上下文污染 | 每 60% token 用 [[Claude_Code_Commands|`/compact`]]，保留决策+未解决项 |
| 安全风险 | deny `.env*` + 安全 scan 进 CI |
| 系统理解流失 | 每重大变更后让 Agent 生成"系统影响说明"写进文档 |
| 协作断层 | 把隐性知识强制写进 AGENTS.md |

---

## 矛盾与争议

- AGENTS.md vs CLAUDE.md 边界：AGENTS.md 定义"能力地图"（如何构建、测试），CLAUDE.md 定义"硬性约束"（不能做什么）。两者不同文件，各司其职，避免混写。

---

## 关联实体

- [[CLAUDE_md_Best_Practices]] — CLAUDE.md 三级分层写法（Global/Project/Local）
- [[Claude_Code_Hooks]] — 确定性验证执行层（postEdit/pre-commit）
- [[Agent_Harness_Engineering]] — Harness 工程完整框架（DECISIONS.md 对应 Compound 阶段）
- [[Harness_Engineering_Deep_Dive]] — AGENTS.md 在 System of Record 方法中的定义与边界
- [[Enterprise_AI_Architecture]] — 企业级 Evals-Driven Development（Golden Dataset）
- [[Claude_Code_Commands]] — Plan-First 四阶段循环（Shift+Tab Plan Mode）
- [[Instruction_Sharing]] — 跨项目共享团队指令文件的 symlink/junction 方案（GitHub Copilot 工作流）

*[Source: raw/AI coding best practice.md]*
