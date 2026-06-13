---
title: MCP Integration Playbook
aliases: ["MCP 集成手册", "Claude MCP 实战", "MCP Hub"]
tags: [mcp, connectors, integration, productivity, tools, workflow]
category: mcp-integration
parent: "[[MCP_Connectors]]"
created: 2026-05-08
date: "2026-05-08"
---

# MCP Integration Playbook

Parent: [[MCP_Connectors]]
Source: [Source: raw/Claude MCP.md]

> 核心论点：MCP 是 Claude 的"感知层扩展"——让 Agent 在不增加代码复杂度的前提下实时访问外部工具数据。优先连接用户已有 SaaS 工具，打造零代码"超级助手"。

---

## 两种快速接入方式（< 5 分钟）

### 方式 1：官方 Connectors UI（推荐）
```
Claude 网页/APP → Customize → Connectors
→ 搜索并一键授权合作伙伴工具
适用：Slack、Notion、Google Drive、Gmail、Calendar
无需代码，即时生效。
```

### 方式 2：自定义 MCP Server
```bash
Claude Desktop → Claude Code → 输入:
claude mcp add
# 粘贴 MCP Server URL + 对应平台 API Key
```
获取 API Key 的方法：提示 Claude Code "帮我找到 [x工具] 免费 API Key 的获取方法"

---

## 12 个高价值 MCP 工具（按优先级）

### 生产力基础（优先安装）
| 工具 | 核心能力 |
|------|----------|
| **Slack** | 搜索全 workspace 消息、发消息、创建 canvas、查看成员 |
| **Notion** | 读写所有 pages/databases/CRM，推送聊天记录想法 |
| **Zapier** | 间接访问 9000+ App，扫描自动化流程 |

### Google 生态（覆盖日常 3 件套）
| 工具 | 示例 Prompt |
|------|-------------|
| **Google Drive** | "从我的 Drive 提取所有 Q3 财务报告的关键洞见" |
| **Gmail** | "总结昨天所有未回复的重要邮件，并起草回复" |
| **Google Calendar** | "基于我本周日历，生成一份每日优先任务列表" |

### 创意工具
- **Excalidraw**：口头描述 → 自动生成手绘架构图
- **Figma**：访问文件并修改设计，生成变体信息图
- **Canva**：批量生成演示文稿或编辑模板

### 金融/专业
- **TradingView**：转为个性化市场助手
- **Perplexity**：增强实时市场/新闻研究（连接后输出质量显著提升）
- **Stripe**：查询收入、交易、失败支付；可 vibe-code 自定义财务仪表盘

### 荣誉提及
Similarweb、Monday/ClickUp、Zoom、Gamma、n8n、Indeed

---

## 高效使用模式

### 专用 MCP Hub Project（强烈推荐）
```
Claude → Projects → New Project → 命名 "MCP Hub"
项目指令：
  你是我的 MCP Hub 助手，已连接以下工具：[列出已连 MCP]。
  每次回答时自动检查哪些 MCP 最相关并直接调用获取最新数据。
  优先使用实时数据，避免猜测。
```

### Vibe-Code 自定义仪表盘（高级）
```
Mega Prompt：
"使用我的 Slack/Notion/Gmail/Calendar/Stripe MCP，
构建一个实时生产力 Dashboard，包含今日任务、未读邮件摘要、
本周会议和收入概览，用 React 风格界面呈现。"
```

---

## AI 应用开发建议

1. 将 MCP 作为 Agent 的"工具调用层"，快速添加外部上下文能力
2. 优先连接用户已有 SaaS 工具，打造个性化超级助手
3. 结合 [[Claude_Cowork]] Projects + 多 MCP → 零代码/低代码生产力系统
4. **成本监控**：MCP 调用计入正常 token 使用，优先用官方 Connectors 获得更好稳定性

---

## 与其他笔记的联系

- [[MCP_Connectors]] — MCP 协议底层架构
- [[MCP_Production_Agent]] — 生产级 MCP Agent 构建
- [[MCP_Production_Decision_Framework]] — 何时用 MCP vs 直接 API
- [[Anthropic_Agent_SDK]] — SDK 层如何调用 MCP 工具
- [[Agent_Harness_Engineering]] — MCP 作为 Harness 工具标准化层
- [[Claude_Optimization]] — 最小化 MCP 调用的 token 成本
- [[MCP_Enterprise_Integrations]] — 企业 MCP 集成（Microsoft Teams + JIRA / Azure AD / Atlassian OAuth）

---

## 矛盾与争议

- MCP vs 直接 API：MCP 加载所有工具定义进上下文（上下文膨胀），单一端点时硬编码更高效（参见 [[Claude_Code_Hacks]] #24）
- 官方 Connectors vs 自定义：官方更稳定但覆盖范围有限；自定义灵活但维护成本高
