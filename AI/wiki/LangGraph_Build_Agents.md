---
title: LangGraph Build Agents（生产级 Agent 构建）
aliases: ["LangGraph Build", "Deep Agents 构建", "State+Nodes+Edges"]
tags: [langgraph, agents, production, state, nodes]
category: agent-engineering
parent: "[[LangGraph_Deep_Agents]]"
created: 2026-05-15
date: "2026-05-15"
---

# LangGraph Deep Agents（生产级 Agent 构建）

Parent: [[LangGraph_Deep_Agents]]
Source: [Source: raw/LangGraph与Deep Agents Build Agents.md, raw/Agent Engineer - 掌握两大核心栈.md]

## 定位
LangGraph 1.0 + Deep Agents 是处理**高复杂度、长程任务**的生产标准框架。核心转变：从"提示工程"向**"Harness Engineering（系统治驭工程）"**的范式转移。

## 核心架构：State + Nodes + Edges

### State Management（持久化状态）
- 定义 `TypedDict` 共享数据容器，存储对话历史、工具输出和任务进度
- 使用 `Annotated` 进行状态合并（Reducer）
- **Checkpointing**（线程持久化）：断点续传、HITL 的基础，支持"时间旅行调试"

### Nodes（功能函数）
每个节点接收当前状态，执行任务并返回状态更新（LLM 调用或工具执行）

### Edges & Routing（执行流）
- `add_edge`：固定边
- `add_conditional_edges`：条件边，根据 LLM 决策或工具结果动态路由
- `Command` 模式（LangGraph 1.0 新特性）：在节点运行期间动态改变图流向

## Deep Agents 增强功能
- **任务规划与分解**：内置 `write_todos` 工具，将复杂目标拆解为可追踪离散步骤
- **子代理生成**：通过 `task` 工具创建上下文隔离的专业子代理，防止"上下文腐烂"，并行探索
- **上下文管理**：文件系统工具（ls/read_file/write_file）将大量数据卸载到磁盘，保持活跃上下文简洁

## 高级编排模式

### Evaluator-Optimizer 模式
生成者与评估者分离：**Generator**（产出）+ **Evaluator**（专门挑刺），结构化反馈循环显著降低幻觉。

### 四阶段循环
```
Plan → Execute → Verify → Repair
```
确保每一个变更都经过真实运行的验证。

### 渐进式知识披露（Progressive Disclosure）
按需加载特定"Skills"或详细指令，避免过长 prompt 导致模型性能下降。详见 [[Claude_Code_Skills]]。

## 记忆系统分层

| 层级 | 实现 | 用途 |
|------|------|------|
| 短时记忆 | LangGraph Checkpointer（MemorySaver） | 中断恢复、时间旅行调试 |
| 情境记忆（Episodic） | 具体历史事件与决策 | 回放能力 |
| 语义记忆（Semantic） | 经过蒸馏的规则、术语 | 领域知识库 |
| 程序化记忆（Procedural） | SOP / Skill 插件 | 重复任务标准化 |

## 两大核心栈学习路径

| 阶段 | 任务 | 关键点 |
|------|------|--------|
| L1：协议层 | 掌握 MCP | 标准化接口连接不同数据源 |
| L2：循环层 | LangGraph 实现自定义 Evaluator | Evals + CI 门禁，防止 Agent 跑飞 |
| L3：系统层 | 构建端到端 Computer Use | Claude SDK 驱动浏览器/本地环境 |

## 可观测性
- **LangSmith**：追踪每一个决策路径和工具调用，调试非确定性行为的必备工具

## 工程建议
- **可靠性胜过聪明**：偏好测试充分的原生 API
- **错误是"主路径"**：保留错误日志在上下文中，让 Agent 自我修正
- 读这两个库的 **Base Class 源码**，重点看 Thread 封装、Context Window 滑动、Short/Long-term Memory 实现

## 关联概念
- [[Agent_Engineer_Roadmap]] — LangGraph 是 Phase 2 的主要工具
- [[Anthropic_Agent_SDK]] — 另一大核心栈
- [[Agentic_Memory_System]] — 记忆分层系统
- [[Human_In_The_Loop]] — LangGraph 中断机制实现 HITL
- [[MCP_Production_Agent]] — MCP 工具连接标准
- [[Agent_Engineer_MOC]] — Agent Engineer 体系学习地图