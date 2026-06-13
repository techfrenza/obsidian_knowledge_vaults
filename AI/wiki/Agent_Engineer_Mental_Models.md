---
title: Agent Engineer Mental Models
aliases: ["Agent 心智模型", "Workflow vs Agent", "增强型LLM"]
tags: [agent, mental-models, workflow, context-primitives]
category: agent-engineer
parent: "[[Agent_Engineer_Roadmap]]"
created: 2026-05-15
date: "2026-05-15"
---

# Agent Engineer Mental Models

Parent: [[Agent_Engineer_Roadmap]]
Source: [Source: raw/Agent Engineer - Mental Model.md]

## 三大核心心智模型

### 1. Workflow vs Agent
决策权归属是本质区分：
- **Workflow**：预定义代码路径编排 LLM 和工具。步骤固定（A→B→C），高可预测性。适用于步骤明确的任务。
- **Agent**：LLM 自主主导处理过程和工具使用。通过[[Agentic_Loop]]运作，动态调整策略。适用于路径不确定的开放式问题。
- **规则**：简单任务用 workflow 保稳定；复杂且路径未知才升级为 agent。

### 2. 增强型 LLM（Augmented LLM）
LLM = 构建代理系统的基础构建块，不只是聊天工具。
- **三大增强**：检索（Retrieval）、工具（Tools）、记忆（Memory）
- **架构比喻**：LLM = CPU，[[Agent_Harness_Engineering|Harness]] = 操作系统，共同构成生产级系统
- 领先模型（Claude 4.x）能主动生成搜索查询、选工具、决定保留什么信息

### 3. 上下文原语（Context Primitives）
上下文是**有限且昂贵的资源**，需通过原语治理（MCP 三原语）：
- **Tools**：模型控制的原语 — 主动调用函数，对外部世界产生副作用（写文件、发请求）
- **Resources**：应用控制的原语 — 只读数据源（DB 快照、文档库），模型可引用不可修改
- **Prompts**：用户控制的原语 — 预设高质量指令模板，引导模型执行专业工作流

### Context is State
上下文 = 系统的**实时状态**，不只是对话历史。
有效[[Context_Engineering]]通过 Compress/Prune/分层治理（CLAUDE.md 永久规则 + Subagents 隔离内存）维持长任务连贯性。
目标：用**最少高信号 Token** 驱动正确行为。

## 关联概念
- [[Agentic_Loop]] — agent 的执行机制
- [[MCP_Production_Agent]] — MCP 协议的生产实践
- [[Claude_Code_Subagents]] — 上下文隔离实现
- [[Context_Engineering]] — 上下文原语的工程化
- [[Karpathy_Methodology]] — Agent 工程方法论的实践来源（4 Rules + Loop + LLM Wiki）

## 矛盾与争议
Workflow vs Agent 边界模糊：复杂程度无统一量化标准，需工程师经验判断。

## 导航
- [[Agent_Engineer_MOC]] — Agent Engineer 体系学习地图
- [[Agent_Engineer_Three_Mental_Models]] — 三大心智模型详解（扩展版，含 MCP 三原语深度解析）