---
title: Multi-Agent 工程系统（Factory Missions）
parent: "[[Multi_Agent_Architecture]]"
tags: [multi-agent, factory, missions, validation-contract, handoff, long-running]
category: agent-engineering
stub: false
date: "2026-06-03"
---

# Multi-Agent 工程系统（Factory Missions）

软件工程的瓶颈已不是智能，而是**人类注意力**。Factory 的 Missions 系统解决如何用 Agent Teams 完成单 Agent 不可能的长期任务。

## 多 Agent 协作五种模式（Luke's Taxonomy）

| 模式 | 描述 | 核心价值 |
|------|------|---------|
| **Delegation** | 父 Agent 派生子 Agent | 最常见，子 Agent 专精化 |
| **Creator-Verifier** | 一 Agent 写代码，另一独立 Agent 审查 | 解决 sunk cost bias |
| **Direct Communication** | Agent 间直接通讯 | 高风险：状态碎片化，无单一真相来源 |
| **Negotiation** | 围绕共享资源协商 | 找正和解法，非互相阻塞 |
| **Broadcast** | 一 Agent 向全体推送约束更新 | 防止长期任务上下文漂移 |

## Factory Missions：三角色架构

### Orchestrator（指挥官）
- 对话中 scope 需求，把模糊目标变成可执行计划
- 实现前必须输出：**Plan**（怎么做）+ **Validation Contract**（什么叫 Done）

### Workers（工人）
- 拿到干净上下文，实现具体特性
- 完成后提交 Git Commit，下一个 Worker 继承**干净代码库**，非混乱聊天记录

### Validators（验证者）
- 不负责帮圆场，严格验证任务是否真正完成
- **Validation Contract 在规划阶段提前写好**（含数百条独立断言）

## 两类验证者

### Scrutiny Validator
- 跑测试、类型检查、Lint
- 为每个特性 spawn 独立 Code Review Agent（全新上下文，无 sunk cost）

### User Testing Validator
- 像 QA 一样启动应用，用 Computer Use 真实点击页面、填写表单
- 验证产品是否真的能用，而非只看代码

## 结构化 Handoff（防止信息丢失）

每个 Worker 完成后填写交接单：
- 完成了什么，留下了什么
- 运行了哪些命令，exit code 是多少
- 发现了什么问题，是否遵守 Orchestrator 流程

里程碑边界：检查所有 handoff，发现未解决问题则自动创建 Follow-up Features（**自愈机制**）。

> 最长 Mission 已跑 **16 天**，目标跑到 30 天。

## 串行主干 + 局部并行

- **特性层面串行**：同一时间只有一个 Worker 推进主干，保证代码库演化连续可理解
- **只读操作并行**：代码搜索、API 调研、Code Review → 高收益低风险

## Mission Control 仪表盘

多天任务专属界面：
- 实时查看当前 Worker 进度、Handoff 摘要、下一步计划
- Token 预算消耗监控
- 完全异步：去睡觉后回来看结构化状态而非聊天记录

## 模型选择策略（Droid Whispering）

| 角色 | 需求 | 推荐模型特性 |
|------|------|------------|
| Planning | 慢思考、战略拆解 | 高智能、慢速 |
| Implementation | 代码流畅、创造力 | 中速、代码能力强 |
| Validation | 精确遵循指令 | 高指令遵从 |

> 不同角色使用不同 Provider，避免同源偏差。开源模型在结构化 Validation Contract 下也能跑得不错。

## 架构哲学：拥抱 Bitter Lesson

Missions 核心逻辑：~700 行文本（prompt + skills），最少硬编码。
- 确定性逻辑：Handoff 检查、阻塞条件、里程碑边界
- 其余智能：留给模型

> 如果把太多智能写死在代码里，模型升级反而吃不到红利。

## 真实案例：Slack Clone

- 60% 时间/Token 花在 Implementation
- Validation 几乎从不一次通过 → 自动创建 Follow-up Features
- 最终代码 50% 是测试，覆盖率 > 90%
- 大量使用 Prompt Caching 控制成本

## 团队吞吐量变化

5 人团队：10 个工作流 → 有了 Missions → **30 个工作流**  
原因：人类从执行解放，转向架构判断、产品决策、验收标准。

## 关联

- [[Multi_Agent_Architecture]] - Multi-Agent 架构概览
- [[Agent_Harness_Engineering]] - Agent Harness 工程
- [[Human_In_The_Loop]] - 人类监督机制
- [[Harness_Engineering_Advanced]] - Harness 进阶指南
- [[AI_Agent_247_Architecture]] - 24/7 可靠运行架构
- [[Production_Reliability_MOC]] - 生产可靠性

[Source: raw/Multi-Agent 工程系统.md]
