---
title: PydanticAI
aliases: ["Pydantic AI", "PydanticAI Framework"]
tags: [pydantic, agent-framework, python, type-safety, SAP]
category: sap-agents
parent: "[[SAP_Agent_Overview]]"
created: 2026-05-24
date: "2026-05-24"
stub: true
---

# PydanticAI

> Pydantic 团队出品的 Python agent 框架，核心价值：**类型安全的工具调用 + 结构化输出**，与 FastAPI/SQLModel 同一生态。在 SAP AppFND agent stack 中与 LangGraph 并列为一阶框架。

## 核心设计

- `result_type=SomeModel` 强制结构化输出（Pydantic BaseModel）
- `@agent.tool` 装饰器声明工具，自动生成 JSON schema
- `TestModel` 内置 mock，无需调用真实 LLM 即可单元测试
- 支持 dependency injection（`RunContext[Deps]`），适合多环境切换

## SAP 使用场景

SAP AppFND SDK 中 PydanticAI 作为轻量 agent 框架首选：
- 意图分类：`result_type=IntentClassification`（见 [[SAP_Agent_Overview]]）
- 输出验证：与 `OutputValidator` 配合（见 [[SAP_Agent_Output_Validation]]）
- 测试：`TestModel` 驱动 behavioral test（见 [[SAP_Agent_Testing]]）

## 关联

- [[SAP_Agent_Overview]] — SAP agent stack 总览
- [[Anthropic_Agent_SDK]] — Anthropic 官方 SDK（对比参考）
- [[LangGraph_Build_Agents]] — 复杂状态机场景的互补选项

[Source: raw/SAP/from-boilerplate-to-production.md]
