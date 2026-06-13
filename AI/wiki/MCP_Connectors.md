---
title: MCP Connectors（模型上下文协议连接器）
aliases: ["MCP Connectors", "MCP 桥梁", "Claude MCP 连接"]
tags: [mcp, connectors, integration, claude, tools]
category: mcp-integration
parent: "[[MCP_Production_Agent]]"
created: 2026-05-15
date: "2026-05-15"
---

# MCP Connectors（模型上下文协议连接器）

Parent: [[MCP_Production_Agent]]
Source: [Source: raw/Anthropic MCP Connectors.md]

## 定义
MCP（Model Context Protocol）是 Claude 官方提供的"桥梁"机制，让 Claude 直接访问外部工具的实时数据并执行操作，无需手动复制粘贴。

## 两种接入方式

### 方式 1：官方 Connectors UI（推荐，< 5 分钟）
Claude 网页/APP → Customize → Connectors → 搜索并授权官方合作伙伴（Slack、Notion、Google Drive、Gmail、Calendar 等）。无需代码。

### 方式 2：自定义 MCP Server
```
Claude Desktop → Claude Code → 输入: claude mcp add
粘贴 MCP 服务器 URL + 对应平台 API Key
```

## 顶级 12 个 MCP 工具（优先级排序）

**生产力基础（优先安装）**
- **Slack**：搜索 workspace、发消息、创建 canvas
- **Notion**：读写所有 pages/databases/CRMs
- **Zapier**：间接访问 9000+ App

**Google 生态（覆盖日常）**
- **Google Drive**：提取财务报告关键洞见
- **Gmail**：读邮件线程、附件、元数据，批量起草回复
- **Google Calendar**：创建/拒绝事件，生成每日优先任务列表

**创意工具**
- **Excalidraw**：口头描述 → 自动生成手绘白板图
- **Figma**：访问文件并修改设计
- **Canva**：批量生成演示文稿

**金融/专业**
- **TradingView**：个性化市场助手
- **Perplexity**：增强实时市场/新闻研究（连接后输出质量显著提升）
- **Stripe**：查询收入、交易、失败支付

## 高效使用模式

### MCP Hub Project（强烈推荐）
```
Claude → Projects → New Project → 命名"MCP Hub"
项目指令：你是我的 MCP Hub 助手，已连接 [工具列表]。
每次回答时自动检查哪些 MCP 最相关并直接调用，优先使用实时数据。
```

### Mega Dashboard Prompt
```
使用我的 Slack/Notion/Gmail/Calendar/Stripe MCP，构建实时生产力 Dashboard，
包含今日任务、未读邮件摘要、本周会议和收入概览，用 React 风格界面呈现。
```

## 开发应用建议
- 将 MCP 作为 Agent 工具调用层，快速为 Claude Agent 添加外部上下文能力
- 优先连接用户已有的 SaaS 工具，打造个性化"超级助手"
- MCP 调用计入正常 token 使用，官方 Connectors 稳定性和速度更好
- 用 `.mcp.json` 版本化配置，改数据源只需改一个文件，无需碰 Skill 或 Agent

## 关联概念
- [[MCP_Production_Agent]] — MCP 生产级决策框架
- [[Anthropic_Agent_SDK]] — Agent SDK 中的 MCP 集成
- [[Agent_Harness_Engineering]] — MCP 作为 harness 的神经系统
- [[Claude_Code_Skills]] — Skills + MCP 配对插件模式
- [[Claude_Cowork]] — Cowork 的 Plugin/Connector 层与 MCP Hub Project 模式语义等价（同一抽象，不同受众）
- [[MCP_Integration_Playbook]] — 12 个工具实战清单 + MCP Hub Project 模板 + Vibe-Code Dashboard 构建
- [[MCP_Enterprise_Integrations]] — 企业 MCP（Microsoft Teams + JIRA）集成，含 Azure AD 应用注册 + Atlassian OAuth
- [[AI_Native_Tool_Design]] — 从 MCP 角度看 AI 原生工具设计：最小工具集、代码执行逃生舱、精确错误码