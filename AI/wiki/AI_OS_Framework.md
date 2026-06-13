---
title: AI OS Framework（Four Cs）
aliases: ["Four Cs", "Claude Code OS", "AI 操作系统", "个人AI操作系统"]
tags: [claude-code, framework, os, four-cs, setup]
category: enterprise-ai
parent: "[[AI_Orchestration_System]]"
created: 2026-05-15
date: "2026-05-15"
---

# AI OS Framework（Four Cs）

Parent: [[AI_Orchestration_System]]

> 将 Claude Code 从聊天工具升级为个人 AI 操作系统的四层搭建框架。[Source: raw/Claude Code OS.md, raw/Claude Code 42个可直接运用的实战Tips.md]

---

## Four Cs 框架（顺序不可逆）

| 层级 | 名称 | 内容 |
|------|------|------|
| C1 | **Context** | About Me / Business / Priorities 文件 |
| C2 | **Connections** | 工具 API 映射（七域：Revenue/Customer/Calendar/Comms/Tasks/Meetings/Knowledge） |
| C3 | **Capabilities** | Skills 可复用 SOP，每个重复任务打包为 skill 或 slash command |
| C4 | **Cadence** | 云/本地例行任务（Pro 计划每天 5 次云例行，laptop 关闭仍运行） |

---

## AI OS 核心理念

- **Wiki 层**（[[Claude_Memory_Layers|Karpathy 方法]]）：`/raw` 放源文件 → Claude ingest 生成 `/wiki` → `_index.md` + `_log.md` + `_hot.md`（500 token 活跃缓存）
- **Three Ms 思维习惯**：Default Shift（任务前问"AI 能做多少"）/ Function Breakdown（拆成可复用树状子任务）/ Curiosity Rule（每次输出后追问 "why"）

---

## 42 条实战 Tips 精华（按层提炼）

### 基础并行与规划（Layer 1）
- `git worktree` 为每个 Claude 实例创建独立目录，5-10 个会话并行不冲突
- `Shift+Tab` 进入 [[Claude_Code_Commands|Plan Mode]] → 完善计划后再 auto-accept → 1-shot 实现
- `PostToolUse Hook` 在 Write/Edit 后自动跑 `bun run format`
- `.claude/settings.json` 预授权常用命令，避免频繁弹窗

### 记忆与定制（Layer 2）
- 每次修正后："Update your CLAUDE.md so you don't make that mistake again."
- 每天做 2 次以上的事都做成 skill 或 slash command，提交 git 共享
- **验证闭环**（最重要）：给 Claude 浏览器扩展/测试套件/日志访问，让它自己验证，质量提升 2-3 倍

### 高级编排（Layer 3）
- `/loop` 长任务持续验证（同一 session 最多 3 天）
- `/rewind`：双 Esc 丢弃失败路径
- `/compact` vs `/clear`：新任务用 `/clear` 手写 brief，`/compact` 做摘要
- `--bare` 模式：SDK 启动速度提升 10 倍
- `git worktree` + 独立任务 + 独立 PR → 真正并行

---

## 每周 Audit + Level Up 循环

- **每周五 /audit**：对 Four Cs 打分，找 Top 3 gaps
- **每周五 /level-up**：五问（重复任务/枯燥事/实习生可做/瓶颈/增长杠杆）
- 成功标志：团队更愿问 AI OS、你少开浏览器、知识离开大脑

---

## Connections 安全接入原则

- 单独 AI 账号 + 受限 API key
- 优先 API endpoint 而非 MCP（节省 token）
- `.env` 存 key，Markdown 存 docs
- 失败即更新 reference doc，形成永久修复

---

## 相关链接

- [[AI_Orchestration_System]] — 100x 工具栈与 Plan-First 三阶段
- [[Claude_Code_Skills]] — Skill / Slash Command 封装
- [[Agentic_Memory_System]] — 四类记忆架构
- [[Claude_Code_Self_Evolving]] — /evolve 自进化循环
- [[Claude_Code_Routines]] — 云端定时任务 Cadence 层
- [[Claude_Code_Advanced_Features]] — CLAUDE.md / Skills / Computer Use / Cloud Routines 的完整实现细节
- [[Agent_Engineer_Roadmap]] — Phase 0–5 路径中，Four Cs 的 Cadence 对应 Phase 5 生产 hardening