---
title: GBrain 架构（Fat Skills + Thin Harness）
parent: "[[GBrain_Architecture]]"
tags: [gbrain, fat-skills, thin-harness, second-brain, garry-tan]
category: enterprise-ai
stub: false
date: "2026-06-03"
---

# GBrain 架构（Fat Skills + Thin Harness）

Garry Tan 的个人 AI 第二大脑构建模板：**Fat Skills + Thin Harness + 100k 页知识图谱**。

## 核心四层架构

| 层 | 说明 |
|----|------|
| **Thin Harness** | 仅路由层（OpenClaw / Hermes Agent / Claude Code Router），数千行代码，只负责"消息 → 匹配 Skill → 执行" |
| **Fat Skills** | 100+ 个独立 Markdown 技能文件，每个只做一件事，存于 Git 仓库 |
| **Fat Data** | 100,000 页结构化知识库（每人/每公司/每会议/每本书独立页面）|
| **模型无关** | Skill 内部决定调用哪个模型（Opus 4.7 做 precision、GPT-5.5 做 recall、DeepSeek 做 creative）|

## 标准目录结构

```
brain-repo/
├── skills/          # 所有 SKILL.md（可 Git PR）
├── people/          # 每人一页
├── companies/       # 每公司一页
├── meetings/        # 每会议一页 + entity propagation
├── books/           # 每本书的 mirror 输出
├── crons/           # 100+ 个每日定时任务
└── resolver.md      # 路由表（Skill 触发条件）
```

## 知识库 Schema（每页必备）

| 区块 | 内容 |
|------|------|
| **Compiled Truth** | 顶部，最新的最佳理解 |
| **Append-only Timeline** | 按时间倒序，所有事件 |
| **Raw Data Sidecar** | 原始来源、transcript、PDF |

## 两大杀手级 Skill

### Meeting-Prep Skill（2 分钟自动生成）
- 拉取目标人物完整 brain page（timeline + current state + open threads）
- 合并最新 biography、podcast、文章
- 输出：3 个 conversation hooks + 3 个 demo scripts + worldview overlap/divergence

### Meeting-Ingestion Skill（会议后）
- 读取 transcript → structured summary
- Entity propagation：遍历所有提到的人/公司，更新 brain page
- 标记 new insights、action items、follow-up

## Skillify 元技能

完成一次重复工作后立即运行 `skillify this workflow`，系统自动：
1. 分析全流程
2. 提取可重复 pattern
3. 生成带 trigger、edge cases 的 SKILL.md
4. 注册到 resolver

## Book-Mirror Skill 示例

Trigger: `book mirror [书名]`
1. 提取全书章节
2. 每章 sub-agent：左侧=作者原意，右侧=映射到个人 brain
3. 跨模型评估（Opus + GPT-5.5 + DeepSeek 互评）
4. 输出双栏 Markdown + PDF
5. 自动更新所有相关 person/company 页面（entity propagation）

## 7 天启动 SOP

1. 安装 OpenClaw / Hermes Agent（或 Claude Code + 文件系统）
2. Fork github.com/garrytan/gbrain（39 个 installable skills）
3. 先建 Book-Mirror：扔一本书，跑 skillify
4. 每天加 3 个 cron（email-triage、calendar-check、media-ingest）
5. 每周跑 cross-modal eval，修复 Skill 中的 factual error

## 核心理念

> **模型是引擎，Skill + Brain 才是车。Fat Skills + Thin Harness + 持续 Skillify = 个人 AI 从玩具到 compounding nervous system。**

## 与已有体系整合

- GBrain 作为 Obsidian 第二大脑的**增强版长期记忆层**。
- 所有 7-Agent Orchestrator 输出自动走 media-ingest → entity propagation。
- MCP Connectors 实时拉 Gmail/Calendar/Slack 喂进 brain。

## 关联

- [[GBrain_Architecture]] - GBrain 核心架构
- [[Skill_Engineering_10_Rules]] - Skill 工程规则
- [[Hermes_Agent]] - Hermes Agent 路由
- [[Harness_Engineering_Deep_Dive]] - Harness 工程
- [[Claude_Code_Skills]] - Skills 生态
- [[Agentic_Memory_System]] - 记忆系统

[Source: raw/GBrain fat skills+Thin Harness.md]
