---
title: Agent Engineer 两大核心栈与学习路径
parent: "[[Agent_Engineer_Roadmap]]"
tags: [agent-engineer, langgraph, claude-sdk, learning-path, roadmap]
category: agent-engineer
stub: false
merge-review: "内容为 [[Agent_Engineer_Learning_Path]] 子集，中优先级考虑合并"
date: "2026-06-03"
---

# Agent Engineer 两大核心栈与学习路径

核心转变：从"顺序流"思维转向**"图/状态机"思维**，并深入底层工程原语。

## 核心栈 1：LangGraph 1.0 + Deep Agents

处理**高复杂度、长程任务**的事实标准，核心是解决 LLM 的非线性逻辑。

### 必掌握技能

**State Management（持久化状态）**
- 定义 `StateSchema`，用 `Annotated` 进行状态合并（Reducer）
- 实现 **Checkpointing**（线程持久化）→ 断点续传、Human-in-the-loop 基础

**循环与条件逻辑**
- `add_edge` 和 `add_conditional_edges` 构建反馈环（Feedback Loops）
- `Command` 模式（LangGraph 1.0 新特性）→ 节点运行期间动态改变图流向

**Deep Agents 架构**
- Multi-agent Teams：通过 `Supervisor` 或 `Hierarchical` 结构隔离 Sub-agents 上下文，防止 Token 爆炸

## 核心栈 2：Claude Agent SDK（Anthropic Control Model）

侧重于 **Computer Use** 和**确定性控制**，理解"模型如何驱动工具"的最佳参考。

### 必掌握技能

**Computer Use 协议**
- `base_64` 屏幕截图处理、坐标转换、交互循环
- `Action Space` 定义 → 构建垂直领域驱动（如自动化运维）的核心

**Tool Use 深度优化**
- "Thought" 链条：调用 Tool 前强制输出推理过程，提高复杂调用准确率
- 错误回溯（Error Back-propagation）：工具返回错误时引导模型自愈

**Harness & Sandboxing**
- `system_prompt` 构建与环境隔离
- `Stream` 处理实时交互

## 三层学习路径

| 阶段 | 核心任务 | 关键点 |
|------|---------|--------|
| **L1：协议层** | 掌握 MCP | 标准化接口连接不同数据源 |
| **L2：循环层** | 在 LangGraph 中实现自定义 Evaluator | Evals + CI 门禁，确保 Agent 不跑飞 |
| **L3：系统层** | 构建端到端 Computer Use 案例 | 驱动浏览器或本地环境完成具体任务 |

## 六阶段完整 Roadmap

| 阶段 | 周期 | 核心任务 | 输出 |
|------|------|---------|------|
| **Phase 0：Foundations** | 1-2 周 | 三大心智模型（workflow vs agent、augmented LLM、context primitives）| 2 页个人文档 |
| **Phase 1：第一个 Agent** | 2-3 周 | 从 scratch 写 100 行 loop，用 Claude SDK 重构 | daily-briefing agent |
| **Phase 2：真实 Agent 架构** | 3-4 周 | LangGraph + Deep Agents 构建 research analyst（parallel sub-agents、PostgresSaver、HITL）| LangSmith trace |
| **Phase 3：自建 Harness** | 3-4 周 | 写 1500 行 mini-harness（loop、tools、compression、hooks、OTEL）| post-mortem 对比 Claude SDK |
| **Phase 4：Eval & 回归 Harness** | 3-4 周 | golden dataset、trajectory evals、LLM-as-judge、CI gating | GitHub Actions block PR + Inspect benchmark |
| **Phase 5：生产加固** | 持续 | cost discipline、latency 优化、sandboxing（Modal/E2B）、prompt caching、model routing | Agent 能在真实用户+真实成本下存活 |

## 核心建议

> 不要只看 API，去读这两个库的 **Base Class 源码**。重点看它们如何封装 `Thread`、如何处理 `Context Window` 滑动以及如何实现 `Short-term vs Long-term Memory`。

## 关联

- [[Agent_Engineer_Roadmap]] - Agent Engineer 完整路线图
- [[Agent_Engineer_Three_Mental_Models]] - 三大心智模型
- [[LangGraph_Build_Agents]] - LangGraph 构建 Agent
- [[LangGraph_Deep_Agents]] - LangGraph Deep Agents
- [[Anthropic_Agent_SDK]] - Anthropic Agent SDK
- [[Human_In_The_Loop]] - HITL 实现

[Source: raw/Agent Engineer - 掌握两大核心栈.md]
