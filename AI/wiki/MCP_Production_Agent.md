---
title: MCP Production Agent
aliases: ["MCP 生产级 Agent", "Model Context Protocol", "MCP 决策框架"]
tags: [mcp, production, agent, api, context-efficient, tool-search]
category: mcp-integration
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# MCP Production Agent

Parent: [[index]]

> 核心论点：MCP（Model Context Protocol）是云端生产 Agent 的首选集成层。通过**工具按需加载**和**程序化 tool calling**，可将上下文消耗降低 85%+。

---

## API / CLI / MCP 三选决策树

| 选择 | 适用场景 | 核心权衡 |
|------|----------|----------|
| **Direct API** | 单 Agent 连单服务、集成不多、不跨平台 | 最快，但规模化产生 M×N 认证问题 |
| **CLI** | 本地开发、沙箱容器、文件系统操作 | 延迟最低，无需额外认证 |
| **MCP** | 云端生产 Agent、跨 web/mobile/cloud、标准化认证 | 生产首选，所有 Claude 兼容客户端自动支持 |

**生产铁律**：三者全部打包发布，MCP 作为云端核心层。

---

## MCP Server 构建模式

### 高阶工具 > 低阶组合
- 不好：`get_thread + parse + create_issue + link`
- 好：`create_issue_from_thread`（一个工具完成全流程）

### 大规模表面：代码编排模式（Cloudflare 官方）
- 只暴露 2 个薄工具：`search` + `execute`
- 让 Agent 自己写脚本覆盖 2500+ endpoints
- 上下文仅 ~1K tokens

### Server Manifest 示例
```json
{
  "tools": [{
    "name": "create_issue_from_thread",
    "description": "从邮件线程创建带附件的 Issue",
    "input_schema": { ... }
  }]
}
```

---

## Context-Efficient Client 模式（多步工作流降耗）

### 工具按需加载
```
开启 tool search → 运行时再查 catalog
效果：上下文减少 85%+
```

### 程序化 tool calling
```python
# 在 sandbox 执行循环/过滤，最后只把最终输出塞回模型上下文
# 多步流程节省 37% tokens
```

### 组合技
`tool search + programmatic calling` = 最小上下文 + 最少 round-trip

---

## 认证标准化

- 用 **Client ID Metadata Documents** + **Claude Vaults**
- 一次注册 OAuth token，后续 session 自动注入刷新

---

## MCP + Skills 配对插件模式

```
Claude Plugin = Skills（流程知识）+ MCP servers（工具访问）
```

示例结构：10 个 Skills + 8 个 MCP servers（Snowflake、Databricks 等）

Server 端直接配送 Skills：让 Agent 不仅知道"能调用什么"，还知道"该怎么用"。

---

## 企业神经系统连接清单

- Git/GitHub（自动建分支、PR 评论）
- Linear/Jira（读票、更新状态）
- Slack（发更新）
- Sentry/Datadog（拉错误日志）
- BigQuery/内部 DB（用真实数据验证假设）
- Confluence/Notion（拉规格和架构决策）

---

## 关联实体

- [[Agent_Harness_Engineering]] — MCP 是 Harness 第二层（工具/神经系统）
- [[Claude_Code_Skills]] — Skills 与 MCP 在 Plugin 中配对
- [[AI_Orchestration_System]] — MCP 是 AI-First 工具栈的神经系统层
- [[Claude_Code_Settings]] — settings.json 配置 MCP connector 权限
- [[MCP_Connectors]] — MCP 产品层配置（官方 Connectors UI）
- [[MCP_Production_Decision_Framework]] — 完整决策框架与最佳实践
- [[Multi_Agent_Architecture]] — 三层架构中的 MCP Connectors 层
- [[MCP_Integration_Playbook]] — 12 工具实战清单与 MCP Hub Project 模板
- [[SAP_Agent_MCP_Integration]] — SAP企业实现：McpServlet + ToolRegistry + 3-Tier路由（AUTO/LOGGED/GATED）+ IAGAGENTTOOLCALL审计表

*[Source: raw/MCP 生产级 Agent 构建决策框架与最佳实践.md, raw/MCP Server Explained.md]*
