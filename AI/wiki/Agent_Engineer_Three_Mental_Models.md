---
title: Agent Engineer 三大心智模型
parent: "[[Agent_Engineer_Mental_Models]]"
tags: [agent-engineer, mental-model, workflow-vs-agent, augmented-llm, context-primitives, mcp]
category: agent-engineer
stub: false
date: "2026-06-03"
---

# Agent Engineer 三大心智模型

在 Anthropic Claude 生态中高效构建应用，必须掌握三个核心心智模型。

## 心智模型 1：工作流 vs 智能体（Workflow vs. Agent）

**本质区别：决策权的归属**

| 维度 | 工作流（Workflows）| 智能体（Agents）|
|------|-------------------|----------------|
| **编排方式** | 预定义代码路径 | LLM 自主主导 |
| **步骤** | 固定（A → B → C）| 动态调整 |
| **可预测性** | 高 | 低 |
| **适用场景** | 任务步骤明确、可拆解 | 路径不确定、需高度灵活 |

**运作机制**：Agent 通过"代理循环"（Agentic Loop）运作：接收任务 → 选择工具 → 执行 → 观察结果并反思 → 动态调整 → 下一轮循环。

> **结论**：简单任务用工作流确保稳定；复杂且路径不确定的任务才升级为智能体。

## 心智模型 2：增强型大语言模型（Augmented LLM）

**定义**：通过**检索（Retrieval）+ 工具（Tools）+ 记忆（Memory）**得到增强的模型。

**架构类比**：
- LLM = CPU（核心处理器）
- Harness（工具接口 + 上下文治理 + 安全拦截）= 操作系统
- 两者共同构成生产级系统

**能力**：Claude 3.5/4.6 系列可主动利用增强能力——自行生成搜索查询、选择工具、决定保留哪些信息。

> **结论**：构建 AI 应用的本质 = 为 LLM 配上"手"（工具）、"眼睛"（检索）和"大脑状态"（记忆）。

## 心智模型 3：上下文原语（Context Primitives）

**核心认知**：上下文是**有限且昂贵的资源**，必须通过特定原语治理。

### MCP 三大原语

| 原语 | 控制方 | 特性 | 用途 |
|------|--------|------|------|
| **Tools** | 模型控制 | 主动调用，产生副作用 | 写文件、发请求 |
| **Resources** | 应用控制 | 只读数据源 | 数据库快照、文档库 |
| **Prompts** | 用户控制 | 预设高质量指令模板 | 专业工作流 slash commands |

### Context is State（上下文即状态）

上下文不仅是对话历史，而是系统的**实时状态**。

有效上下文工程需要：
- **压缩（Compaction）**：定期折叠历史
- **剪枝**：移除无关信息
- **分层治理**：
  - `CLAUDE.md` = 永久规则
  - Subagents = 隔离内存

> **结论**：治理上下文的目标 = 用最少的高信号 Token 驱动模型产生正确行为。

## 关联

- [[Agent_Engineer_Mental_Models]] - Agent Engineer 心智模型概览
- [[Agent_Engineer_MOC]] - Agent Engineer 知识地图
- [[Agentic_Loop]] - 代理循环详解
- [[Context_Engineering]] - 上下文工程
- [[Contextmaxxing]] - 上下文最大化
- [[MCP_Integration_Playbook]] - MCP 集成策略

[Source: raw/Agent Engineer - Mental Model.md]
