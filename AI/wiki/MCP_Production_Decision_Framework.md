---
title: MCP 生产级 Agent 构建决策框架
aliases: ["MCP 决策框架", "API vs CLI vs MCP", "MCP 决策树"]
tags: [mcp, decision-framework, api, cli, production]
category: mcp-integration
parent: "[[MCP_Production_Agent]]"
created: 2026-05-15
date: "2026-05-15"
---

# MCP 生产级 Agent 构建决策框架

Parent: [[MCP_Production_Agent]]
Source: [Source: raw/MCP 生产级 Agent 构建决策框架与最佳实践.md]

## API / CLI / MCP 三选决策树

```
项目启动前 1 分钟判断：

单 Agent 连单服务 + 集成不多 + 不跨平台
    → Direct API（最快，但规模化产生 M×N 认证问题）

本地开发 + 沙箱容器 + 文件系统操作 + 测试调试
    → CLI（延迟最低，无需额外认证，直接用 credential 文件）

云端生产 Agent + 需跨 web/mobile/cloud + 需标准化认证 + 多客户端复用
    → MCP（生产首选）

铁律：三者全部打包发布，MCP 作为云端核心层，其他作为补充。
```

## MCP Server 构建模式

### 工具设计原则
- **按意图分组，而非按 endpoint 镜像**：高阶工具 > 低阶组合
  - Good：`create_issue_from_thread`（从邮件线程直接创建带附件的 Issue）
  - Bad：`get_thread` + `parse` + `create` + `link`（4 个低阶工具）
- **大规模表面用代码编排（Cloudflare 官方模式）**：只暴露 2 个薄工具（`search` + `execute`），让 Agent 自己写脚本覆盖 2500+ endpoints，上下文仅 ~1K tokens

### 交互式返回
- 用 MCP Apps 返回 inline UI（图表、表单、仪表盘）
- Claude.ai / Cowork 原生支持

### 用户输入暂停（Elicitation）
- Form mode → 渲染原生表单
- URL mode → 跳转浏览器 OAuth
- 避免 Agent 猜参数导致的幻觉

### 认证标准化
- Client ID Metadata Documents + Claude Vaults
- 一次注册 OAuth token，后续 session 自动注入刷新

## Context-Efficient Client 模式（降低 85% 上下文消耗）

| 技术 | 效果 |
|------|------|
| 工具按需加载（tool search） | 运行时再查 catalog，上下文减少 85%+ |
| 代码内处理结果（programmatic tool calling） | sandbox 执行循环/过滤，只把最终输出塞回模型；多步流程节省 37% tokens |
| 两者组合 | 最小上下文 + 最少 round-trip，适合多 Server 并行 |

## MCP + Skills 配对插件模式
将 Skills（流程知识）+ MCP servers 打包成一个插件（含 hooks、LSP、subagents）：
- 示例：10 个 Skills + 8 个 MCP servers（Snowflake、Databricks 等）
- Server 端直接配送 Skills：Agent 不仅知道"能调用什么"，还知道"该怎么用"
- Canva、Notion、Sentry 已在 Claude directory 发布

## 生产立即行动清单
1. 新建 MCP server，按意图分组先写 2-3 个高阶 tool manifest
2. 部署 Cloudflare / 自建 server，测试 tool search + programmatic calling
3. 把现有重复流程打包成 Skill，注册进同一个 Plugin
4. 生产任务启动前先跑一次决策树，强制走 MCP + Vaults
5. 每周 review 一次 context 使用量，用上述模式迭代

## 关联概念
- [[MCP_Connectors]] — MCP Connectors 的产品层面配置
- [[Claude_Code_Skills]] — Skills + MCP 配对插件模式
- [[Context_Engineering]] — 上下文效率优化
- [[Anthropic_Agent_SDK]] — Agent SDK 中的 MCP 集成
- [[AI_Orchestration_Practice]] — .mcp.json 版本化配置
- [[AI_Native_Tool_Design]] — AI 原生工具设计三约束（无记忆/不能浏览/需精确）与设计检查清单