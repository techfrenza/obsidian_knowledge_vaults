---
title: SAP Agent Guardrails MCP
aliases: ["GuardedMCPToolset", "MCP-level Guardrails"]
tags: [SAP, guardrails, MCP, agent-safety, AppFND]
category: sap-agents
parent: "[[SAP_Agent_Guardrails]]"
created: 2026-05-24
date: "2026-05-24"
stub: true
---

# SAP Agent Guardrails MCP

> MCP 工具由 API spec 生成并跨 Agent 共享，因此**护栏不能放在 MCP Server 侧**。
> 解法：`GuardedMCPToolset` 中间件——包裹 `MCPServerStreamableHTTP`，在 Agent 侧注入 per-agent 规则。

## 架构位置

```
Agent → GuardedMCPToolset (agent-specific guardrails) → MCPServerStreamableHTTP → S/4HANA OData
```

## 核心接口

- `EnforceableRule` interface：`evaluate(tool_args, ctx) → RuleResult`
- 规则按 `get_order()` 排序，第一条失败即终止链
- `AmountLimitRule`：回溯 `ctx.messages` 历史工具响应，在执行 cancel/write 前校验金额

## 典型规则示例

| 规则 | 检查点 | 触发条件 |
|------|--------|---------|
| `AmountLimitRule` | 写操作前 | 金额超过阈值 |
| `ReadOnlyRule` | 所有工具 | agent 角色为只读 |
| `FieldMaskRule` | 读操作 | 过滤敏感字段（PII）|

## 关联

- [[SAP_Agent_MCP_Integration]] — MCP 集成全景（含 GuardedMCPToolset 代码架构）
- [[SAP_Agent_Guardrails]] — Agent 级别的 6 层防御体系
- [[SAP_Agent_Overview]] — SAP agent stack 总览

[Source: raw/SAP/mcp-integration-deep-dive.md]
