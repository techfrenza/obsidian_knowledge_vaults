---
title: AI Orchestration Practice
aliases: ["围绕AI构建系统实践", "Orchestration Practice", "AI编排实战"]
tags: [orchestration, ai-coding, workflow, practice, tools]
category: agent-engineering
parent: "[[AI_Orchestration_System]]"
created: 2026-05-15
date: "2026-05-15"
---

# AI Orchestration Practice（围绕 AI 构建系统）

Parent: [[AI_Orchestration_System]]
Source: [Source: raw/AI Orchestration Practical Knowledge 围绕AI构建系统.md]

## 核心转变
从"让 AI 写代码"转向**"围绕 AI 构建系统"**。
- 你是架构师，不是写手
- AI 是力量倍增器，你严格拥有架构、验证和约束
- 编排（Orchestration）而非委托（Delegation）：AI 并行工作，你只做决策和审查

## 5 层工具栈

| 层级 | 工具 | 用途 |
|------|------|------|
| AI-First IDE | Cursor / Windsurf / VS Code + Copilot Agent | 小编辑、样板、重构、修测试 |
| Terminal-First Agent | Claude Code / Open Code / Gemini CLI | 长上下文仓库分析、多文件重构、运行命令 |
| Background Agents | OpenAI Codex、Google Jules、Cursor BG、Devin | 异步委托：你睡觉时它们工作 |
| General Chat | Claude / ChatGPT / Gemini（浏览器） | 高层次推理、设计文档、复杂调试 |
| AI Code Review | Codium PR-Agent、GitHub Copilot Workspace、What-The-Diff | 永不跳过的代码审查 |

## Persistent Context 系统（停止 Prompt Hacking）
在仓库根目录建 `CLAUDE.md` + 扩展上下文文件夹：
```
/business-info    # 策略、产品约束、SLA
/writing-styles   # 语气规范
/examples         # 最佳 PR、API 设计、理想测试
/agents           # 子代理角色定义
```

Prompt 模板：
> "使用 /examples/best-auth-flow 模式实现此功能，遵守 CLAUDE.md 的安全规则，使用 /business-info/cost-model.md 的定价约束。"

## MCP 神经系统（.mcp.json 版本化）
统一配置，所有 AI 工具共享同一"神经系统"：
- Git/GitHub → 自动建分支、PR 评论
- Linear/Jira → 读票、更新状态
- Slack → 发更新
- Sentry/Datadog → 拉错误日志
- BigQuery/内部 DB → 用真实数据验证假设

## Parallel Agents 工作流（Boris Cherny 模式）
同时开 5-10 个独立会话，每个有独立上下文：
```
Session 1：实现 Feature A
Session 2：写 Feature B 的测试和文档
Session 3：数据库迁移
Session 4：重构 auth 模块
Session 5：调查生产 bug
```
你循环审查，只在需要决策时介入。**这是编排，不是多任务。**

## Plan-First 执行流程（3 阶段）
1. **Spec**（人类 + 聊天模型）：澄清真实问题、约束、非谈判项。列所有 edge cases → 提出 2-3 种架构 + 明确权衡 → 你选一种
2. **Plan**（Coding Agent）：分步实施计划，列出精确文件、修改函数、要写的测试
3. **Execution**（带 auto-accept 的 Agents）：仅在计划批准、分支创建、上下文可用后切换 auto-edit

## 5 条核心原则
1. 所有 AI PR 都是你的 PR，你拥有 bug 和后果
2. 可靠性 > 聪明：偏好无聊、测试充分的原生 API
3. 系统思考：每次本地优化都问"10x 规模下会怎样？"
4. 先框问题再解决方案
5. 约束管理是核心学科：把预算、SLA、限额编码进 CLAUDE.md

## Night Queue（Background Agents 最佳实践）
建立 `night_queue.md`，下班前踢 3-5 个低风险重构任务给 background agents，早晨只审 PR。

## 关联概念
- [[CLAUDE_md_Best_Practices]] — Persistent Context 系统的核心文件
- [[MCP_Production_Agent]] — MCP 神经系统的生产实践
- [[Claude_Code_Subagents]] — Parallel Agents 的实现机制
- [[Agent_Harness_Engineering]] — Harness 工程的系统架构
- [[AI_Team_Coding_Practice]] — AI 编码团队实践
- [[AI_Workflow_System]] — Workflow-First 分类框架（🟢/🟡/🔴 可重复任务的系统化视角）