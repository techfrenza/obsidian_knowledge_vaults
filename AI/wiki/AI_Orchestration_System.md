---
title: AI Orchestration System
aliases: ["围绕AI构建系统", "100x 工具栈", "AI-First 架构", "Plan-First 执行"]
tags: [orchestration, ai-coding, parallel-agents, plan-first, night-queue]
category: agent-engineering
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# AI Orchestration System

Parent: [[index]]

> 核心论点：核心转变是从"让 AI 写代码"转向"围绕 AI 构建系统"。你是架构师，AI 是力倍增器。使用**编排**而非委托——让 AI 并行工作，你只做决策和审查。

---

## 现代 100x 工具栈（5 层）

| 层级 | 工具 | 用途 |
|------|------|------|
| AI-First IDE | Cursor / Windsurf / VS Code + Copilot | 小编辑、样板、重构 |
| Terminal Agent（主力） | **Claude Code** | 长上下文仓库分析、多文件重构 |
| Background Agents（秘密武器） | OpenAI Codex / Google Jules / Devin | 异步委托，睡觉时工作 |
| General Chat | Claude / ChatGPT / Gemini | 高层次推理、设计文档 |
| AI Code Review | Codium PR-Agent / What-The-Diff | 自动 PR 审查，阻塞严重问题 |

---

## Plan-First 三阶段执行流程

### Phase 1: Spec（人类 + 聊天模型）
- 澄清真实问题、约束、非谈判项
- 列所有 edge cases → 提出 2-3 种架构 + 权衡 → 你选一种

### Phase 2: Plan（Coding Agent）
```
根据此 spec，提出跨仓库的分步实施计划。
列出将触碰的精确文件、修改的函数、要写的测试。
```
迭代直到计划尊重现有架构、有检查点、有回滚策略。

### Phase 3: Execution（带 auto-accept 的 Agents）
只在计划批准、分支创建、上下文可用后切换 auto-edit。  
**范围漂移就停止，返回 Phase 1。**

---

## Parallel Agents 工作流

同时开 5-10 个独立会话，每个独立上下文：
- Session 1: Feature A 实现
- Session 2: Feature B 测试和文档
- Session 3: 数据库迁移
- Session 4: auth 模块重构
- Session 5: 生产 bug 调查

---

## Night Queue 系统

下班前建 Markdown 文件，踢 3-5 个后台任务：
```markdown
# Night Queue 2026-04-30
- [ ] 修复所有 eslint 警告并开 PR
- [ ] 迁移 payments 模块弃用 API
- [ ] 生成本周依赖安全扫描报告
```

---

## 5 条核心原则（避免变成 vibe coder）

1. 所有 AI PR 都是**你的** PR，你拥有 bug 和后果
2. 可靠性 > 聪明：偏好无聊、测试充分的原生 API
3. 系统思考：每次本地优化都问"10x 规模下会怎样？"
4. 先框问题再解决方案
5. 约束管理是核心学科：把预算、SLA、限额编码进 [[CLAUDE_md_Best_Practices|CLAUDE.md]]

---

## Verification & Background Agents

- **测试优先**：AI 列所有 edge cases → 写 property-based tests → 先审测试再看实现
- **双重审查**：AI 处理风格/一致性/文档；人类处理架构/安全/可维护性
- **Background Agents 任务要求**：一个 PR 范围 + 清晰验收标准 + 约束 + [[CLAUDE_md_Best_Practices|CLAUDE.md]] 链接

---

## 关联实体

- [[Agent_Harness_Engineering]] — Harness 是 Orchestration System 的技术实现层
- [[MCP_Production_Agent]] — AI 工具栈的神经系统连接层
- [[Claude_Code_Subagents]] — 并行 Agent 的 Claude Code 原生实现
- [[Claude_Code_Routines]] — Background Agents 的云端自动化形式
- [[CLAUDE_md_Best_Practices|CLAUDE.md Best Practices]] — Persistent Context System 的核心文件
- [[AI_Workflow_System]] — 面向非开发者的 Workflow-First 业务自动化实现
- [[AI_Orchestration_Practice]] — 本页的实战延伸：5 层工具栈与 Parallel Agents 操作清单

*[Source: raw/AI Orchestration Practical Knowledge 围绕AI构建系统.md, raw/AI coding best practice.md]*
