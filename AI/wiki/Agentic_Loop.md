---
title: Agentic Loop（代理循环）
aliases: ["代理循环", "Agent Loop", "四阶段循环"]
tags: [agent, agentic-loop, anthropic-sdk, execution-model]
category: agent-engineering
parent: "[[Anthropic_Agent_SDK]]"
created: 2026-05-15
date: "2026-05-15"
---

# Agentic Loop（代理循环）

Parent: [[Anthropic_Agent_SDK]]
Source: [Source: raw/Anthropic 代理循环 (Agentic Loop).md]

## 定义
代理循环是 Anthropic 代理系统的**核心执行机制**。它使 Claude 超越传统"请求-响应"模式，通过独立思考、使用工具并根据反馈自我修正来完成复杂任务。

## 四阶段结构

```
任务解析 → 工具选择 → 自主执行 → 观察与反思 → [循环]
```

1. **任务解析（Task）**：接收用户指令（如"修复项目所有类型错误"），转化为可执行目标
2. **工具选择（Tool Selection）**：Claude 自主决定所需工具（LS 查文件、Read 读代码等）
3. **自主执行（Execution）**：SDK 自动处理工具调用，开发者无需手写分发逻辑
4. **观察与反思（Observation & Reasoning）**：根据工具返回的**"地面事实"**（代码执行结果、错误日志）评估进度；任务未完成则动态调整策略，开启下一轮循环

## vs. 传统工作流

| 维度 | Workflow（工作流） | Agentic Loop（代理循环） |
|------|-------------------|------------------------|
| 决策权 | 预定义代码路径 | LLM 自主主导 |
| 路径 | 固定（A→B→C） | 动态调整 |
| 适用场景 | 步骤明确的任务 | 路径不确定的开放式问题 |
| 成本 | 低（单次调用） | 高（多轮循环） |
| 风险 | 低 | 错误可累积，需 HITL 机制 |

## 关键特性
- **自主性**：LLM 自主决定如何达成目标，适用于无法预知具体步骤的问题
- **代价**：多次调用 → 成本和延迟高于单次请求，可能产生错误累积
- **安全建议**：在受控沙盒环境中运行，加入[[Human_In_The_Loop|人工确认（HITL）]]机制

## 关联概念
- [[Anthropic_Agent_SDK]] — SDK 中 agentic loop 的实现
- [[Agent_Harness_Engineering]] — 围绕 loop 构建的 harness 工程
- [[Harness_Engineering_Deep_Dive]] — Harness 对 loop 的五大控制机制（Recitation/Physical Blocks/渐进自主权）
- [[Human_In_The_Loop]] — loop 中的人工干预机制
- [[Claude_Code_Subagents]] — loop 的并行化通过子代理实现
- [[Context_Engineering]] — loop 运行期间的上下文治理
- [[LangGraph_Build_Agents]] — LangGraph 中 loop 的状态机实现（State/Nodes/Edges + Evaluator-Optimizer 模式）

## 矛盾与争议
高自主性与成本控制之间存在张力：loop 运行轮数越多，成本越高，需通过 evals + 成本门控约束。

## 导航
- [[Agent_Engineer_MOC]] — Agent Engineer 体系学习地图
- [[Loop_Engineering]] — Loop 工程学实践方法论：本页技术机制的工程化上位（Open/Closed Loop + 6构建块 + 14步路线图）