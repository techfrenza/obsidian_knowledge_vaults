---
title: Claude Cowork
aliases: ["Cowork", "Cowork Plugins", "Claude Cowork 工作空间"]
tags: [cowork, plugins, connectors, slash-commands, automation, workspace]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Cowork

Parent: [[index]]

> 核心论点：Claude Cowork 是独立于 Claude Code 的产品——面向非开发者的 **业务自动化平台**。核心升级是从"问问题"变为"委托完整 outcome"，通过 Plugins + Connectors + Slash Commands + Sub-agents 四层构建专属团队。

---

## 产品定位（vs Claude Code）

| 维度 | Claude Cowork | [[Claude_Code_Skills|Claude Code]] |
|------|--------------|------|
| 受众 | 业务/运营人员 | 开发者 |
| 核心抽象 | Plugins + Connectors | [[Claude_Code_Skills|Skills]] + [[Claude_Code_Hooks|Hooks]] |
| 触发方式 | `/slash command` | Skill 描述触发 |
| 数据连接 | OAuth Connectors | [[MCP_Production_Agent|MCP]] / CLI |
| 典型任务 | 邮件分类、会议简报、内容产出 | 代码审查、多文件重构、CI 自动化 |

---

## 四层核心架构

```
Plugin = Skills（专属指令）
       + Connectors（实时数据：Gmail/Notion/Slack/Drive）
       + Slash Commands（/command 触发全流程）
       + [[Claude_Code_Subagents|Sub-agents]]（并行执行）
```

一条 `/command` 直接出成品，取代：手动连工具 + 贴 prompt + 管子流程。

---

## 11 个官方免费插件

| 插件 | 核心命令 |
|------|---------|
| Sales | `/sales:call-prep`、`/sales:account-plan`、`/sales:objection-handling` |
| Marketing | `/marketing:seo-audit [URL]`、`/marketing:email-sequence`、`/marketing:competitive-brief` |
| Legal | `/legal:contract-review`、`/legal:compliance-check` |
| Finance | `/finance:model`、`/finance:forecast` |
| Customer Support | `/support:draft-response`、`/support:escalate` |
| Product Management | `/product:write-prd`、`/product:prioritize-roadmap` |
| Data Analysis | `/data:clean-dataset`、`/data:visualize` |
| Enterprise Search | `/search:company-wide` |
| Biology Research | `/research:literature-review` |
| Productivity | `/productivity:prepare-meeting` |
| Plugin Create | `/plugin:create-new`（一键生成自定义插件） |

开源地址：`github.com/anthropics/knowledge-work-plugins`

---

## Workspace 文件结构（必建）

```
/Cowork-Workspace
├── about-me.md           ← 姓名、角色、公司、时区、沟通风格
├── brand-voice.md        ← 常用词、禁用词、示例句子
├── current-projects.md   ← 活跃项目、截止日期、blockers
├── successful-examples/  ← 最佳邮件/帖子/提案（反向学习）
├── workflows/
├── briefings/
├── outputs/
└── archives/
```

`successful-examples/` 是关键——让 Cowork 从高绩效输出中学习个人风格。

---

## 全局 Meta-Prompt（永久生效，复制使用）

```
Always start with a clear plan before any action.
Read about-me.md, brand-voice.md and current-projects.md first.
Confirm understanding of the task.
Only proceed after I approve the plan.
Never modify files outside approved folders.
```

---

## 委托公式（每次任务的 Prompt 结构）

```
What needs to happen（具体动作）
+ Where（精确路径/应用）
+ How output（格式/命名/结构）
+ What to avoid（约束）
+ Edge cases（异常处理）
```

示例：
> "Organize /Projects into subfolders by client name... Never move files without clear match. For ambiguous files put in /Unsorted."

---

## 5 个必建定时自动化（[[Claude_Code_Routines|/schedule]]）

| 时间 | 任务 |
|------|------|
| 每天 7am | Morning Briefing：Gmail + Calendar + Slack → 2min briefing.md |
| 每天 6pm | End-of-Day Log：今日修改文件 → completed/in-progress/next |
| 周五 5pm | File Hygiene：处理 /Downloads，移动+删除 60 天旧文件 |
| 周五 4pm | Weekly Report：汇总 Daily log → weekly summary |
| 每月 1 号 9am | Finance：Receipts 图片 → 分类 Excel + totals |

---

## 三个高级闭环系统

### Content Production
Voice note → transcribe → research → draft article → 拆成 tweet/LinkedIn/email → 存 `/Content/[date]`

### Client Management
会议前：Gmail + Drive + Slack → prep brief  
会议后：笔记 → extract action + draft follow-up + 更新 client folder

### Weekly Intelligence（周一 8am）
web search competitor + internal data → one-page changed highlights

---

## 7 步正确上手顺序

1. 建 3 个核心上下文文件（about-me / brand-voice / current-projects）
2. 设置全局 Meta-Prompt
3. Week 1 只建 1 个 Workflow（Meeting Prep 推荐）
4. 建文件夹结构
5. 安装插件（Productivity → 行业插件 → 自定义插件）
6. 设置 1 个 Scheduled Task（晨会简报）
7. 每周 Friday 10min Review Loop（更新上下文 + 追加 examples + 调整 prompt）

---

## 关联实体

- [[AI_Orchestration_System]] — Cowork 是 Orchestration System 的非开发者实现
- [[Claude_Code_Skills]] — Skill 封装与 Cowork Plugin 的类比关系
- [[Claude_Code_Routines]] — Routines 与 /schedule 的技术对应
- [[Cross_Platform_Memory]] — about-me/brand-voice 是 Cowork 版的跨平台记忆系统
- [[Managed_Agent_Memory]] — Connectors 提供 Cowork 的实时外部记忆
- [[Claude_Projects_Power_Usage]] — Projects 是 Cowork 的持久化工作区层（25 技巧方法论）

*[Source: raw/Claude Cowork.md]*
