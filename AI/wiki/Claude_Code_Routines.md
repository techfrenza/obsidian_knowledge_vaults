---
title: Claude Code Routines
aliases: ["自动化 Routines", "Claude Routines", "定时任务"]
tags: [routines, automation, schedule, github-trigger, claude-code]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Code Routines

Parent: [[index]]

> 核心论点：Routines 是运行在 Anthropic 云端的自动化任务，无需本地 cron 或 GitHub Actions YAML。适合"夜间窗口"型批处理——issue triage、PR review、deploy 验证。

---

## Trigger 类型

| Trigger | 适用场景 |
|---------|----------|
| **Schedule** | 定时任务（weekdays daily，允许几分钟延迟）|
| **API** | 监控工具/CI 直接调用 `/fire` endpoint |
| **GitHub** | PR 打开/推送时自动触发 |

三者可同时启用。

---

## 核心 Prompt 写作规则

1. **写死 "done" 输出形式**：Slack 消息、draft PR、labeled issue
2. **明确指定 connector 名称**：`#dev-standup`、Linear 项目 ID
3. **异常处理语句**："If anything unexpected, post error summary to #dev-alerts and stop."
4. 不要写 "Check for issues."，要写完整动作链条

---

## 三个即用模板

### 每日凌晨 Backlog 清理（schedule trigger）
```
Read all GitHub issues opened today in {repo},
apply a label from [bug, feature, docs, question, needs-triage] to each,
assign it based on which files it references,
and post a summary to #dev-standup with the count and breakdown.
```

### 自动 PR 代码审查（GitHub trigger）
```
Review this PR for security, performance, and style issues.
For each finding, add an inline comment with severity (high/medium/low)
and suggested fix. If clean, post 'LGTM - all checks passed' to the PR.
```

### API 触发（curl 示例）
```bash
curl -X POST https://api.anthropic.com/v1/claude-code/routines/{routine_id}/fire \
  -H "Authorization: Bearer {your_token}" \
  -H "Content-Type: application/json" \
  -H "anthropic-beta: experimental-cc-routine-2026-04-01" \
  -d '{"text": "Alert ID 123 fired in prod: high error rate on /api/users"}'
```

---

## GitHub Trigger 注意事项

- 必须完成两步：`/web-setup`（授权 clone）+ 安装 Claude GitHub App（webhook）
- filter 用 `contains` 而非 regex
- 默认只能 push `claude/-` 前缀分支；需在设置开启 "Allow unrestricted branch pushes"
- 每小时上限超出**直接丢弃**（非排队），filter 一定要窄

---

## 配额与限制

- 每天 run 上限在 `claude.ai/code/routines` 和 `claude.ai/settings/usage` 查看
- Routine 属于**个人账号**，commit/PR 以你 GitHub 身份出现，无法团队共享（预览期）
- Team/Enterprise 开启 metered overage 可超额计费，其他计划超限直接拒绝

---

## 关联实体

- [[Claude_Code_Skills]] — Skill 是交互式触发，Routines 是无人值守自动化
- [[Claude_Code_Settings]] — settings.json 配置触发权限
- [[Agent_Harness_Engineering]] — Routines 是 Harness 的异步执行扩展
- [[AI_Orchestration_System]] — Routines 是 Background Agents 的云端实现
- [[Claude_Cowork]] — Cowork 的 `/schedule` 定时任务是非开发者侧的对等机制
- [[Skill_Design_Patterns]] — Pipeline 模式与 Routines 的 step-checkpoint 结构互补

*[Source: raw/Claude Code Routines.md, raw/Claude Code Routine.md]*

---

## /schedule 命令操作指南（2026年5月最新）

### 创建方法（最简单）

```bash
claude  # 进入会话后输入：
/schedule daily at 9am run my-daily-skill
# 或更完整的自然语言：
/schedule every day at 8:00 AM execute the Process-Inbox skill, then generate morning brief, and save output to /00-INBOX/brief-{{date}}.md
```

Claude 会交互式询问：Prompt细节、仓库权限、MCP Connectors、时间配置。

**完成后**：Routine 持久运行在 Anthropic 云端，**电脑关机不影响**。

### 自包含 Prompt 模板（精确调用 Skill）

```
每天早上9:00自动运行。严格执行以下步骤：
1. 调用 /Process-Inbox Skill 处理00-INBOX文件夹
2. 调用 /Generate-Brief Skill 生成今日简报
3. 把所有输出保存到正确文件夹
4. 通过Slack MCP 发送总结通知

使用我的CLAUDE.md和所有可用Skills。
如果Inbox为空，则只发送"今日无新内容"通知。
```

触发 Skill 的写法：直接写 `/skill-name` 或自然描述 "run the Process-Inbox skill"。

### 管理命令

| 命令 | 功能 |
|------|------|
| `/schedule list` | 查看所有 Routine |
| `/schedule run [name/ID]` | 立即测试运行 |
| `/schedule update [name]` | 更新 Routine |
| 网页管理 | `claude.ai/code/routines`（CLI 暂不支持删除） |

### 关键区别：云端 vs 本地

| 类型 | 触发方式 | 持久性 | 适用场景 |
|------|---------|--------|---------|
| **云端 Routine**（`/schedule`） | Anthropic 云调度 | 永久，电脑关机仍运行 | 日常定时任务 |
| **本地 /loop** | 本地 Claude Code 进程 | 需 Claude Code 保持开启，3天后自动过期 | 临时单次轮询 |

**配额（Pro/Max）**：每天 Routine 运行次数有限额（Max 约 15次/天）；Skill 必须在项目或全局 Skill 目录中可用。

*[Source: raw/Claude Code Routine.md]*

---

## Claude Cowork Workflow 构建（Role+Tools+Trigger+Output 框架）

> Chat 是"一问一答"，Workflow 是"有固定岗位的数字员工"——按计划/事件自动运行、调用工具、产出成品，你只负责审批。

[Source: raw/How to Build Claude Workflows That Run Without You.md]

### 四要素框架

| 要素 | 作用 | 示例 |
|------|------|------|
| **ROLE** | 岗位描述（System Prompt），固定角色行为 | "你是我的晨间内容趋势分析师" |
| **TOOLS** | 工具权限（Connectors），定义可访问资源 | Gmail、Google Calendar、Web Search |
| **TRIGGER** | 启动条件 | Schedule（每天 7am）或 Event（新邮件到达）|
| **OUTPUT** | 定义交付物 | 发消息、存文件、草稿邮件（先留人工审批）|

**第一个 Workflow 用 Schedule，比 Event 更简单，价值最高。**

### 5 步构建流程（30 分钟上手）

1. **写 Role + Goal（最重要）**：给它清晰的岗位描述，"你是 [角色]，你的任务是每 [周期] 产出 [成果]"
2. **连接 Tools**：只连接这个任务真正需要的工具（文件/邮件/日历/Web Search）
3. **设置 Trigger**：Schedule（定时）或 Event（事件触发）—— 第一个选 Schedule
4. **定义 Output**：发消息/存文件/草稿邮件（不可逆操作留人工审批直到信任建立）
5. **测试 + 迭代**：手动跑一次，修 Prompt（3-4 轮后 Workflow 按你的标准稳定运行）

### Morning Briefing Workflow（即用模板）

```
You are my morning briefing workflow

Every morning at 7am, send me one message with three sections:

1. TODAY: my calendar for the day, flag any meeting that needs prep
2. INBOX: only the emails that actually need a reply today, skip newsletters and noise
3. SIGNAL: one thing that happened in [your niche] in the last 24 hours, two lines max

Rules:
- one message, no preamble, no sign off
- if a section is empty, say so in one line and move on
- never pad it with filler, I want the shortest version that's still complete
```

在 Claude Cowork 中：Settings → Connectors 启用 Gmail + Google Calendar + Web Search → Scheduled Tasks → 粘贴 Prompt → 设置每天 7am → 手动运行一次验证 → 开启 Schedule。

### 角色场景快速参考

| 角色 | Workflow | 节省时间 |
|------|---------|---------|
| 内容创作者 | Trend（5-7 个带角度 post idea）/ Repurpose（一文多平台改写）| 45 分钟/天 |
| 自由职业者 | Lead Qualifier / Client Report / Sales Call Prep | 1-4 小时/任务 |
| 研究员 | Daily Digest / Company Teardown / Keyword Monitor | 1-2 小时/天 |
| 通用 | Morning Briefing / File Organizer / Follow-up | 30 分钟/天 |

### 与 Claude Code Routines 的关系

| 维度 | Claude Code Routines | Claude Cowork Workflow |
|------|---------------------|----------------------|
| 用户 | 开发者（CLI 配置）| 非开发者（GUI 构建）|
| 连接 | GitHub/Slack/Linear via MCP | Gmail/Calendar/Web via Connectors |
| 配置 | `/schedule` 命令 + settings.json | Scheduled Tasks UI |
| 持久性 | Anthropic 云端，电脑关机仍运行 | 同上 |

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图