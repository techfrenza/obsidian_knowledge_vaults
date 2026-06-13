---
title: LangGraph & Deep Agents
aliases: ["LangGraph 1.0", "Deep Agents", "LangChain Agent Runtime", "Agent Protocol"]
tags: [langgraph, deep-agents, langchain, orchestration, workflow-patterns, persistence]
category: agent-engineering
parent: "[[index]]"
created: 2026-05-06
date: "2026-05-06"
---

# LangGraph & Deep Agents

Parent: index

> 核心论点：LangGraph 是有状态多角色协同的运行时，Deep Agents 是构建在其上的生产级线束层。真正的瓶颈不是模型性能，而是 [[Agent_Harness_Engineering|Harness]] 的设计质量。

---

## LangGraph 1.0：核心编排运行时

将 LLM 调用建模为**图节点（Nodes）**，决策逻辑建模为**图边（Edges）**，通过 `StateGraph` 同时支持确定性工作流与自主智能体模式。

### 三大核心能力

| 能力 | 机制 | 实际价值 |
|------|------|----------|
| **持久化与断点续传** | 内置 Checkpointing，`thread_id` 区分线程 | 长时任务上下文不丢失，支持时间旅行调试 |
| **人机回环（HITL）** | 高风险工具调用前设置物理 Interrupt | 生产环境安全审批的基础设施 |
| **双模态** | Workflow（预定义路径）vs Agent（LLM 自主决策）| 在确定性逻辑与自主决策间取得平衡 |

> 关联：[[Human_In_The_Loop]] — HITL 在金融风控/系统安全的具体应用场景

---

## Deep Agents：生产级 Harness 组件包

独立库，专为复杂多步骤任务设计，提供标准化"组件包"。

### 核心组件

| 组件 | 功能 |
|------|------|
| `write_todos` 工具 | 将大目标拆解为离散步骤，动态调整计划 |
| 上下文治理 | 文件系统工具（`ls`/`read_file`/`write_file`）脱离大块上下文到存储 |
| 子智能体生成 | 父 Agent 孵化专门子 Agent 处理子任务，实现上下文隔离 |
| 异步子智能体（v0.5+）| 在远程服务器独立运行，不阻塞主线程，适合分钟级长时任务 |
| 沙箱后端 | 虚拟文件系统支持内存/本地磁盘/Modal/Deno，确保代码执行安全 |

---

## 五种工作流模式（覆盖 80% 任务）

| 模式 | 结构 | 适用场景 |
|------|------|----------|
| **提示链** | 线性，每步输出→下步输入 | 结构化文档处理 |
| **并行化** | 同时多 LLM 调用 | 多维度分析、多方案投票 |
| **路由** | 模型分类→专门处理节点 | 意图分发 |
| **编排者-工人** | Orchestrator 动态拆分→Workers 执行→综合结果 | [[AI_Orchestration_System|Deep Research]] 类系统核心模式 |
| **评估者-优化者** | 生成→评估→反馈→循环 | 翻译/代码质量提升 |

---

## 治驭工程最佳实践（Harness Engineering）

- **三角色架构**：Planner + Generator + Evaluator，防止长时运行漂移
- **渐进式披露**：给 Agent 一份 ≤100 行的 `[[AI_Team_Coding_Practice|AGENTS.md]]` 作目录，按需检索 `docs/references/`，禁止塞千页说明书进 Prompt
- **确定性验证**：关键节点设硬性拦截 [[Claude_Code_Hooks|Hooks]]（如 pre-commit Linter），物理阻挡优于推理建议
- **自进化循环**：犯错后将教训写回 `DECISIONS.md`，定期将[[Agentic_Memory_System|情境记忆（Episodic）]]蒸馏为[[Agent_Context_Architecture|语义记忆（Semantic）]]

> 关联：Agent_Harness_Engineering — 完整 Harness 六层架构与防 Rot 策略

---

## 协议与互操作性

| 协议 | 用途 |
|------|------|
| **MCP（Model Context Protocol）** | 无需自定义代码连接数据源（PostgreSQL/Notion/Slack）|
| **Agent Protocol** | LangChain 开放标准，跨平台 Server↔Agent 通信，支撑异步子智能体 |
| **AP2（Agent Payments Protocol）** | 涉及金钱或高风险操作的非对称授权证明（Mandates）+ 审计轨 |

> 关联：[[MCP_Production_Agent]] — MCP 生产级架构决策框架

---

## 关联实体

- Agent_Harness_Engineering — Harness 六层架构（LangGraph 对应运行时底层）
- Human_In_The_Loop — HITL Interrupt 机制的具体实现
- MCP_Production_Agent — MCP 协议在跨工具互操作中的应用
- AI_Orchestration_System — 多 Agent 编排系统整体架构
- [[Claude_Code_Subagents]] — Claude Code 子代理编排与 Deep Agents 异步子智能体对比
- [[LangGraph_Build_Agents]] — LangGraph 生产级 Agent 构建实战（State/Nodes/Edges/Evaluator-Optimizer）
- [[Agent_Engineer_Roadmap]] — Phase 2 用 LangGraph 构建 research analyst
- [[Agent_Engineer_MOC]] — Agent Engineer 体系学习地图
- [[AI_Agent_Payments]] — AP2 协议的落地实现：x402（AWS Bedrock AgentCore）是 AP2 思想的 HTTP-native 具现

*[Source: raw/LangGraph 1.0 与 Deep Agents (LangChain).md]*
