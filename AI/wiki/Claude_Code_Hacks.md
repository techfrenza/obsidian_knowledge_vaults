---
title: Claude Code Hacks
aliases: ["CC Hacks", "Claude Code 技巧", "Claude Code 速通"]
tags: [claude-code, hacks, productivity, workflow, context-management, subagents]
category: claude-tooling
parent: "[[Claude_Code_MOC]]"
created: 2026-05-08
date: "2026-05-08"
---

# Claude Code Hacks

Parent: [[Claude_Code_MOC]]
Source: [Source: raw/Claude Code hacks.md]

> 核心论点：最大收益不来自更好的 prompt，而来自**工作流架构**——更干净的上下文、更小的任务切片、自检循环、专用廉价模型、精准的权限配置。

---

## Beginner（1-10）：防止 90% 的差输出

| # | 技巧 | 核心操作 |
|---|------|----------|
| 1 | `/init` 每个项目 | Claude 扫描代码库写 CLAUDE.md，无需重复解释 |
| 2 | 状态栏 `/statusline` | 底部显示模型、上下文%、费用 |
| 3 | 保持上下文精简 | 不要倾倒整个代码库，分步聚焦 |
| 4 | `/context` 找 token 膨胀 | 精准定位吃 token 的来源 |
| 5 | **60% 时 `/compact`** | 指定保留内容；切换任务用 `/clear` |
| 6 | 从 Plan Mode 开始 | `Shift+Tab` 切换模式，先映射方案再写代码 |
| 7 | 当 junior dev 使用 | 描述问题而非下命令，激发 chain of thought |
| 8 | 让 Claude 主动提问 | "保持问直到 95% 确信"——减少返工轮次 |
| 9 | 截图验证 | 自检闭环：截图 → 确认 → 继续 |

---

## Intermediate（11-22）：4x 速度提升

| # | 技巧 | 核心操作 |
|---|------|----------|
| 11 | 并行 Sub-agents | 每个独立上下文，主线程保持干净 |
| 12 | 构建 Custom Skills | `.claude/skills/` 存可复用 SOP，一行命令触发，提交 git 团队共享 |
| 13 | 子任务用 Haiku | 廉价研究 + 大上下文消耗，摘要返回主线程 |
| 14 | 持续更新 CLAUDE.md | 记录新模式和坑，防止重复错误 |
| 15 | CLAUDE.md 路由到其他文件 | 保持 < 200 行，链接到风格指南/参考文档 |
| 16 | 走偏立即 ESC 重开 | 不要烧 token 看它失败 |
| 17 | 激进挑战输出 | "推翻重来，做更优雅的版本" → 更新 skill |
| 18 | `/rewind` 快速撤销 | 回滚到上一个节点 |
| 19 | Hooks 通知 | 会话结束后声音提醒，同时跑 15 个会话只看响的 |
| 20 | 截图输入 | 错误界面、参考设计、半成品网站直接喂给 Claude |
| 21 | Chrome DevTools | Claude 点按钮、填表单、测试 UI 功能 |
| 22 | 克隆参考网站 | 截图 → "做成这个样子" |

---

## Pro（23-32）：专业级工作流

| # | 技巧 | 核心操作 |
|---|------|----------|
| 23 | **Git Worktrees 并行** | 同项目独立分支，同时跑 4-5 个会话互不覆盖 |
| 24 | API endpoint 代替 MCP | MCP 加载所有工具定义进上下文；只需单一端点时直接硬编码 |
| 25 | `/loop` 周期任务 | "每 5 分钟检查部署"，同会话最长跑 3 天（见 [[Claude_Code_Routines]]） |
| 26 | VPS 常驻会话 | SSH 接入，Telegram 触发，笔记本关闭 Claude 继续工作（见 [[AI_Agent_247_Architecture]]） |
| 27 | 手机远程控制 | 桌面启动重任务，咖啡馆用手机指挥 |
| 28 | BigQuery CLI 数据分析 | 自然语言查询，无 SQL 代码 |
| 29 | **Ultrathink** | 输入关键词，分配最大推理预算（~32K tokens）用于架构/重构/顽固 Bug |
| 30 | 权限精细化 | allow 安全操作，deny 破坏性操作；deny 列表永远优先（见 [[Claude_Code_Settings]]、[[Claude_Code_Security]]） |
| 31 | Agent Teams | 子代理互相通信 + 共享任务列表 + 自动分工 |
| 32 | [[MCP_Production_Decision_Framework|Context7 MCP]] | 拉取最新库文档，避免幻觉过期 API |

---

## 核心心智模型

```
收益来源优先级：
工作流架构 > 权限配置 > 上下文管理 > prompt 质量
```

- **上下文就是注意力**：60% 强制 compact，切换任务 clear
- **失败快经济学**：污染上下文后修复成本 = 重开成本 × 2-3
- **廉价模型路由**：认知步骤 1-2 → Haiku；5+ → Sonnet；10+ → Ultrathink（参见 [[Opus_4_7_Migration]] 模型选择策略）

---

## 与其他笔记的联系

- [[Claude_Code_MOC]] — Claude Code 体系总图
- [[Claude_Code_Subagents]] — 子代理上下文隔离详解
- [[Claude_Code_Skills]] — `.claude/skills/` 可复用 SOP 设计
- [[Claude_Code_Hooks]] — Hooks 通知与强制检查机制
- [[Context_Engineering]] — 上下文治理完整框架
- [[Agent_Harness_Engineering]] — 工作流架构背后的工程原理
