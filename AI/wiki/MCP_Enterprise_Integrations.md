---
title: MCP Enterprise Integrations（Teams & JIRA）
aliases: ["企业 MCP 集成", "Teams MCP", "JIRA MCP", "Microsoft Graph MCP"]
tags: [mcp, enterprise, microsoft-teams, jira, atlassian, integration]
category: mcp-integration
parent: "[[MCP_Integration_Playbook]]"
created: 2026-05-20
date: "2026-05-20"
---

# MCP Enterprise Integrations（Teams & JIRA）

Parent: [[MCP_Integration_Playbook]]
Source: [Source: raw/Claude Code 连接 Microsoft Teams 和 JIRA 完整指南.md]

> 核心论点：企业 MCP 集成（Microsoft Teams + JIRA）与消费级 SaaS MCP 有本质区别——前者需要 Azure AD 应用注册/OAuth 授权，存在管理员权限壁垒；后者只需 API Token。两类工具形成"**个人助理**"闭环：Teams 扫描对话提取决策 → JIRA 跟踪行动项。

---

## Microsoft Teams MCP 集成

### 方案一：Microsoft Graph API MCP（推荐，企业 M365 环境）

**前提**：需要 M365 管理员开放 API 权限。

```bash
# 安装社区 MCP Server（Microsoft Graph）
claude mcp add msgraph -- npx -y @pash1986/mcp-server-ms-graph

# 或使用 Teams 专用 MCP
claude mcp add teams -- npx -y mcp-teams-server
```

**Azure AD 应用注册步骤**：
1. Azure Portal → Azure Active Directory → App registrations → New registration
2. 申请权限：`ChannelMessage.Read.All`、`Team.ReadBasic.All`、`Channel.ReadBasic.All`
3. 获取：`tenant_id`、`client_id`、`client_secret`

**`.mcp.json` 配置**：
```json
{
  "teams": {
    "command": "npx",
    "args": ["-y", "mcp-teams-server"],
    "env": {
      "TENANT_ID": "your-tenant-id",
      "CLIENT_ID": "your-client-id",
      "CLIENT_SECRET": "your-client-secret"
    }
  }
}
```

**权限注意事项**：
- `ChannelMessage.Read.All` 属于高权限，通常需要管理员审批
- 若只读自己的消息，使用委托权限（delegated）+ 用户登录，权限要求更低
- SAP 企业环境需单独确认 M365 管理员是否已开放 API 权限

### 方案二：官方 Microsoft 365 Connector（Claude Team/Enterprise 计划）

```
Claude 网页 → Organization settings → Connectors → Add Microsoft 365 → 授权登录
```

支持：Channel Messages、Chat History、Meeting Transcripts（零代码）

### 方案三：直接调用 Microsoft Graph REST API（无需 MCP）

```bash
# 获取 token
curl -X POST "https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token" \
  -d "client_id=...&client_secret=...&scope=https://graph.microsoft.com/.default&grant_type=client_credentials"

# 读取频道消息
curl -H "Authorization: Bearer {token}" \
  "https://graph.microsoft.com/v1.0/teams/{team_id}/channels/{channel_id}/messages"
```

---

## JIRA MCP 集成

### 方案一：Atlassian 官方 Remote MCP（推荐，JIRA Cloud）

```bash
claude mcp add atlassian --transport sse https://mcp.atlassian.com/v1/sse
```

首次连接触发 OAuth 浏览器授权流程（无需手动配置 API Token）。

**常用 JQL 查询 Prompt**：
```text
# 查看所有未关闭工单
用 JQL: assignee = currentUser() AND resolution = Unresolved ORDER BY updated DESC

# 过去 24 小时有更新的工单
用 JQL: assignee = currentUser() AND updated >= -1d ORDER BY updated DESC

# 当前 Sprint 中我的工单
用 JQL: assignee = currentUser() AND sprint in openSprints()
```

### 方案二：本地 MCP Server（JIRA Server / 私有部署）

```bash
# 生成 API Token：Atlassian → Account Settings → Security → Create API tokens
claude mcp add jira -- npx -y @smithery/mcp-atlassian
```

**`.mcp.json` 配置**：
```json
{
  "jira": {
    "command": "npx",
    "args": ["-y", "@smithery/mcp-atlassian"],
    "env": {
      "JIRA_URL": "https://your-company.atlassian.net",
      "JIRA_EMAIL": "your-email@company.com",
      "JIRA_API_TOKEN": "your-api-token"
    }
  }
}
```

### 方案三：直接 REST API（快速临时查询）

```bash
curl -u "email:api_token" \
  "https://your-company.atlassian.net/rest/api/3/search?jql=assignee%3DcurrentUser()%20AND%20resolution%3DUnresolved"
```

---

## 方案选型矩阵

| 场景 | 推荐方案 |
|------|----------|
| 公司 JIRA Cloud（个人使用） | Atlassian 官方 MCP + OAuth |
| 公司 JIRA Server（私有部署） | @smithery/mcp-atlassian + API Token |
| Microsoft Teams（企业内部） | Microsoft Graph API MCP + Azure AD 应用 |
| 快速临时查询 | 直接 Bash + REST API（无需配置 MCP） |

---

## 运维优化

### 持久化配置（避免重复设置）

在项目根目录创建 `.mcp.json` 文件，重启 Claude Code 自动加载。

### CLAUDE.md 规则示例

```markdown
# Teams & Jira Rules
- 扫描 Teams 时优先提取 action items 和 decisions
- Jira 查询时只关注 assignee = me 的 tickets
- 输出使用中文结构化格式
```

### 状态检查

```bash
claude /mcp status   # 检查 MCP 连接状态
claude /doctor       # 诊断连接问题
```

---

## 矛盾与争议

- **官方 Connector vs 自建 MCP**：官方 M365 Connector 零代码但限 Claude Team/Enterprise 计划；自建 Graph API MCP 更灵活但需 Azure AD 注册（管理员壁垒）
- **Teams MCP 数据范围**：委托权限只读自己参与的频道；应用权限（Application permissions）可读全部频道，但需更高管理员审批等级
- **SAP 企业环境特殊性**：SAP 使用 Microsoft 365 + Azure AD，但 API 权限开放政策受 SAP IT 安全策略约束（参见 [[SAP_Agent_Guardrails]] 中的企业安全层）

---

## 关联概念

- [[MCP_Integration_Playbook]] — 消费级 SaaS MCP 集成（Slack/Notion/Google）
- [[MCP_Connectors]] — MCP 协议底层架构与接入方式
- [[MCP_Production_Decision_Framework]] — 何时用 MCP vs 直接 API
- [[Claude_Code_Settings]] — `.mcp.json` 持久化配置与权限管理
- [[SAP_Agent_Guardrails]] — 企业 MCP 安全层（GuardedMCPToolset）
- [[Human_In_The_Loop]] — 敏感操作（创建 Ticket、发消息）需 HITL 审批
- [[Agentic_Loop]] — 企业 MCP 中"何时暂停等人确认"的底层决策逻辑
- [[Claude_Code_Hooks]] — MCP 配置持久化与 hooks 协作（postEdit 触发 mcp status 检查）
- [[SAP_Agent_Joule_Integration]] — IAS App2App 双向授权与 Azure AD App2App 是同一问题的 SAP 解法（身份联邦）
- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图
