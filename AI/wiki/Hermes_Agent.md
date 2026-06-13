---
title: Hermes Agent
aliases: ["Hermes", "Hermes AI", "自进化 Agent"]
tags: [hermes, open-source, agent, self-improving, cron, telegram]
category: agent-engineering
parent: "[[GBrain_Architecture]]"
created: 2026-05-15
date: "2026-05-15"
---

# Hermes Agent

Parent: [[GBrain_Architecture]]

> 核心论点：Hermes 是一个**自进化**的开源 Agent，每 15 次工具调用后自动提炼技能文件，是 Claude Code 的**移动端补充层**（而非替代品）——在口袋里 24/7 运行的计划/自动化层。

---

## 定位 vs Claude Code

| | Hermes | Claude Code |
|---|---|---|
| 使用场景 | 移动端 Telegram、定时任务、语音交互 | 桌面端代码开发、深度重构 |
| 特点 | 24/7 自动运行（VPS/Mac Mini） | 离开 laptop 即离线 |
| 技能积累 | 每 15 次工具调用自动写入可复用 skill 文件 | 手动维护 SKILL.md |
| 模型接入 | 200+ 模型（via OpenRouter / Anthropic API / Ollama） | 主要 Claude 系列 |

**共生关系**：同一大脑（Claude），不同界面。Hermes 处理"在路上"的任务、定时摘要、服务器健康检查；Claude Code 处理桌面端深度工程工作。

---

## 五大支柱（Five Pillars）

### 1. Memory（跨 Session 记忆）
- 存储用户偏好、项目上下文、对话历史
- 类似 `~/.claude/memory/`，但以 Hermes 专属格式持久化
- 支持 Namespace 隔离（不同项目独立记忆）

### 2. Skills（可复用技能库）
- 出厂 91 个内置 Skill；社区 Hub 额外 520+ 个
- 16 个 Anthropic 官方 Skill（内含 Excalidraw 白板生成、转录等）
- 安装示例：`hermes skill install <name>`
- 自动学习：每 15 次工具调用 → 分析本次 Session → 写入新 Skill 文件（核心差异化）

### 3. Soul（角色人格文件）
- 定义 Agent 的性格、沟通风格、价值观
- 等价于 CLAUDE.md 的"Identity + Guardrails"章节
- 一次配置后跨所有 Channel 生效

### 4. Crons（定时任务）
- 用自然语言或 YAML 定义周期性任务
- 示例用例（真实跑通）：
  - 每日 AI 新闻摘要 → 推送 Skool 社区
  - YouTube 评论监控 + 自动回复（带讽刺但不粗鲁的语调）
  - 早晨商业摘要（含服务器健康检查）
  - 社区成员互动管理
- 配置位置：`.hermes/crons/` 目录

### 5. Self-Improving Loop（自学习循环）
- 核心机制：每 15 次工具调用触发反思
- Agent 自问："这次 Session 有什么可以复用的模式？"
- 自动生成 `.skill` 文件并追加到技能库
- 随时间推移，Agent 在高频任务上效率指数级提升

---

## 安装与部署

### 三种部署方式

| 方式 | 成本 | 优点 | 缺点 |
|------|------|------|------|
| **VPS（推荐）** | ~$4-5/月（Hostinger） | 24/7 在线，一键模板 | 需要 Linux 基础 |
| **本地 Mac/Linux** | 免费（电费） | 私密，Ollama 离线跑 | 关机即离线 |
| **托管平台** | 收费 | 零配置 | 集成受限 |

### 快速安装（60 秒）
```bash
# Linux / macOS / WSL2
curl -fsSL https://hermes.sh | bash

# 启动配置向导
hermes setup
```

*注意：原生 Windows 不支持完整安装，需先装 WSL2。*

### 模型选择
```
推荐入门组合：OpenRouter + Qwen 3.6（低成本，快速）
Claude 直连：Anthropic API Key
本地离线：Ollama（Gemma 4 / Llama 3）
```

---

## 连接渠道

Hermes 支持多端接入：
- **Telegram**（最常用，支持语音消息 → 文字转录）
- WhatsApp
- Discord
- Slack
- Signal

配置步骤（以 Telegram 为例）：
1. 创建 Telegram Bot → 获取 Token
2. `hermes connect telegram --token <TOKEN>`
3. 开始对话

---

## 真实运行案例（@nateherk）

```
我的主 Hermes 每天运行：
→ AI 日报摘要 → 推送 Skool 社区
→ YouTube 评论监控 + 讽刺风格回复
→ 早晨商业数据摘要
→ 服务器健康检查
→ 社区成员互动
```

---

## 与 GBrain 体系的关系

- Hermes 的 Skills = [[Claude_Code_Skills]] 的 Skill 概念在 Hermes 生态的实现
- Hermes 的 Self-Improving Loop = [[GBrain_Architecture]] 的 Skillify 自动化形态
- Hermes 的 Soul = [[CLAUDE_md_Best_Practices]] 的 Identity/Guardrails 层
- Hermes Crons ≈ [[Claude_Code_Routines]] 的云端定时任务（但部署在自托管 VPS 而非 Anthropic 云）

---

## 矛盾与争议

- **Hermes vs Claude Code 边界**：两者功能有重叠（均支持工具调用、技能系统）；但实践中定位互补而非竞争——Hermes 用于低延迟移动端触达，Claude Code 用于高质量桌面端工程。
- **自学习可靠性**：自动生成的 Skill 文件质量取决于 Session 的代表性；低质量 Session 可能生成噪声 Skill，需定期 review。

---

## 关联概念

- [[GBrain_Architecture]] — Hermes 是 GBrain "在口袋里"的移动延伸
- [[Claude_Code_Skills]] — 类比 Skill 系统（Hermes 版本有自进化能力）
- [[Claude_Code_Self_Evolving]] — Claude Code 侧的 Corrections→Rules 自进化闭环（与 Hermes 15-call skill 提炼互为参照）
- [[Claude_Code_Routines]] — Cron 的 Anthropic 托管版（两者解决同一问题，不同基础设施）
- [[Agentic_Memory_System]] — Hermes 的跨 Session 记忆 = Agentic Memory 的轻量实现
- [[AI_Agent_247_Architecture]] — Hermes 24/7 可靠性模式的具体平台实例
- [[Claude_Cowork]] — Cowork 是 Anthropic 生态的桌面协作层，Hermes 是开源社区的移动自动化层
- [[SAP_Agent_Durable_Execution]] — Hermes Crons（持久化定时任务）与 SAP Durable Execution（持久化工作流）是同一"跨 pod 重启保持状态"问题的不同实现；Hermes 用 VPS cron，SAP 用 LangGraph/Temporal

*[Source: raw/Easiest Step-by-step Hermes agent guide - Setup + Workflow.md, raw/From Zero to Ultimate Hermes Agent Army.md]*
