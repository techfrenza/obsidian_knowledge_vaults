---
title: Agent Engineer Learning Path
parent: "[[Agent_Engineer_MOC]]"
tags: [agent-engineering, learning-path, langchain, claude-sdk]
category: agent-engineer
stub: false
date: "2026-06-03"
---

# Agent Engineer Learning Path

6阶段完整学习路径，从基础心智模型到生产级 Agent 工程。

## 三大核心心智模型（Phase 0 基础）

### 1. Workflow vs Agent 决策原则
- **Workflow**：预定义代码路径编排 LLM + 工具，步骤固定（A→B→C），高可预测性
- **Agent**：LLM 自主主导处理过程 + 工具使用，通过 Agentic Loop 动态调整策略
- **决策规则**：简单任务 → Workflow（稳定）；路径不确定的复杂任务 → 升级为 Agent

### 2. 增强型 LLM（Augmented LLM）
- LLM = CPU；围绕它构建的 [[Harness_Engineering_Deep_Dive|Harness]] = 操作系统
- 增强三元素：**检索（Retrieval）+ 工具（Tools）+ 记忆（Memory）**
- 架构角色：LLM 是引擎，Harness 提供工具接口、上下文治理、安全拦截

### 3. 上下文原语（Context Primitives）
- [[Context_Engineering|MCP 三大原语]]：
  - **Tools**：模型控制，产生外部副作用（写文件、发请求）
  - **Resources**：应用控制，只读数据源（数据库快照、文档库）
  - **Prompts**：用户控制，预设高质量指令模板
- **Context is State**：上下文是系统实时状态，目标是通过最少高信号 Token 驱动正确行为

## 6阶段 Roadmap

| 阶段 | 时长 | 核心任务 | 输出 |
|------|------|---------|------|
| Phase 0 | 1-2周 | 建立三大心智模型（workflow/agent/context） | 2页个人文档 |
| Phase 1 | 2-3周 | 从 scratch 写100行 loop，再用 Claude Agent SDK 重构 | daily-briefing agent |
| Phase 2 | 3-4周 | LangGraph + Deep Agents 构建 research analyst（并行子 Agent + PostgresSaver + HITL） | LangSmith trace |
| Phase 3 | 3-4周 | 写1500行 mini-harness（loop + tools + compression + hooks + OTEL） | post-mortem vs Claude SDK |
| Phase 4 | 3-4周 | golden dataset + trajectory evals + LLM-as-judge + CI gating | GitHub Actions PR block + Inspect benchmark |
| Phase 5 | ongoing | cost discipline + latency优化 + sandboxing + prompt caching + model routing | 生产 Agent 存活真实用户和成本 |

## 双核技术栈

### LangGraph 1.0 + Deep Agents（复杂长程任务标准）
- `StateSchema` + `Annotated` Reducer 进行状态合并
- **Checkpointing**：线程持久化，实现断点续传、HITL
- `Command` 模式：节点运行期间动态改变图流向
- Deep Agents：Supervisor 架构隔离子 Agent 上下文，防 Token 爆炸

### Claude Agent SDK（确定性控制）
- Computer Use 协议：`base_64` 截图 + 坐标转换 + Action Space 定义
- **"Thought" 链条**：Tool call 前强制输出推理过程，降低幻觉
- 错误自愈：Error Back-propagation 引导模型自愈

## 学习路径（L1-L3）

| 层级 | 核心任务 | 关键点 |
|------|---------|-------|
| L1：协议层 | 掌握 MCP | 标准化接口连接不同数据源 |
| L2：循环层 | LangGraph 中实现自定义 Evaluator | Evals + CI 门禁，防止 Agent 循环跑飞 |
| L3：系统层 | 构建端到端 Computer Use 案例 | Claude SDK 驱动浏览器/本地环境 |

**核心建议**：不要只看 API，读 Base Class 源码。重点看 `Thread` 封装、`Context Window` 滑动机制、Short-term vs Long-term Memory 实现。

## 5-Test 框架（What to Learn/Skip）

在采纳任何新框架前运行5项测试：
1. **2年后是否重要**？包装器 → 否；原语（协议/内存模式/沙箱方案）→ 是
2. **有没有受尊重的团队写过生产 postmortem**？
3. **采用是否要求丢弃现有 tracing/auth/config**？→ 是则框架试图成为平台，90% 死亡率
4. **跳过6个月的成本**？大多数为零 → 等6个月看清楚
5. **能否衡量它是否对你的 Agent 有帮助**？没有 Evals = 靠直觉跑回归

### 框架推荐（2026年4月）
- **LangGraph**：生产默认，状态机思维，约1/3大型公司在用
- **Pydantic AI**：TypeScript 外选 Mastra；Pydantic 优先选 PydanticAI（v1.0 2025年底）
- **避开**：AutoGen（已交社区维护）、CrewAI（易 demo 难生产）、Semantic Kernel（除非 MS 锁定）

## 关联知识
- [[Context_Engineering]] 上下文工程深度课程
- [[LangGraph_Deep_Agents]] LangGraph 状态机详解
- [[Harness_Engineering_Deep_Dive]] Harness 构建原则
- [[Harness_Over_Model_Principle]] Harness 重于模型的公理（78% vs 42% 实证）
- [[Agent_Engineer_Roadmap]] 职业发展路径
- [[Production_Agent_Engineering]] 生产 Agent 四大原语
- [[Agent_Engineering_Primitives]] 持久原语清单 + 5-Test过滤器
- [[Agentic_Loop]] Agent 运行循环机制
- [[Claude_Code_Subagents]] Claude Code 子代理实现

[Source: raw/Agent Engineer - Mental Model.md]
[Source: raw/Agent Engineer - 掌握两大核心栈.md]
[Source: raw/Grok and Gemini Chats.md]
[Source: raw/What to Learn, Build, and Skip in AI Agents.md]
