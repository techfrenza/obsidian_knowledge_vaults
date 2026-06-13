---
title: Enterprise AI Architecture
aliases: ["企业 AI 架构", "MCP 多智能体闭环", "Agentic 公司架构"]
tags: [enterprise, architecture, mcp, langgraph, evals, guardian-agents, state-machine]
category: enterprise-ai
parent: "[[index]]"
created: 2026-05-01
date: "2026-05-01"
---

# Enterprise AI Architecture

Parent: index

> 核心论点：企业级 AI 系统的重心从"写 Prompt"转向**工具设计（Tool Design）与上下文工程（Context Engineering）**。采用状态机架构 + MCP 统一连接层 + Guardian Agents 安全层，实现可靠、可回溯的多智能体闭环。

---

## 三层核心架构（放弃顺序流，采用状态机）

### 连接层（MCP）
- 统一用 [[MCP_Production_Agent|Model Context Protocol]] 替代碎片化 API 适配
- 直接挂载 GitHub、ERP、本地数据库——"即插即用"
- 多客户端共享同一上下文，无需手动复制粘贴

### 逻辑层（Orchestration）
- 生产默认框架：**[[LangGraph_Deep_Agents|LangGraph]]**（状态机 + Typed Dict + Reducers）
- 模式：Orchestrator-Subagent — 主控分发任务，子代理在**隔离 Context** 中执行
- 隔离执行防止长上下文引发的逻辑幻觉
- 关联：[[AI_Orchestration_System]] — Parallel Agents 工作流与 Plan-First 三阶段

### 执行层（Sandbox）
- 所有 Tool-calling 必须在 **E2B** 或 **Browserbase** 等沙箱中运行
- 确保执行过程安全且可回溯（Replay）

---

## 数据与记忆：Agentic RAG

| 机制 | 做法 |
|------|------|
| [[Agent_Context_Architecture|Context Engineering]] | 每个 Loop 前执行 Summarize & Prune，不依赖无限长文本 |
| 状态持久化 | File-system-as-State：每步 Think-Act-Observe 记录为结构化文件（唯一真理来源）|
| 跨 Session 记忆 | 引入 **Mem0** 轻量化用户偏好记忆，只在跨 Session 决策时检索 |

> 关联：[[Agentic_Memory_System]] — 四类内存架构详细实现

---

## 安全治理：Guardian Agents（守门员）

```
正常执行流
    ↓
Guardian Agent（安全分类器）
    ├── 低风险：自动放行
    └── 高风险（删除/转账）→ HITL 拦截 → 人工确认
```

- **实时审计**：通过 **Langfuse** 记录完整 Tracing，决策逻辑透明化
- **HITL 节点**：高风险工具必须符合预设 Golden Dataset 安全边界才可释放
- 关联：[[Claude_Code_Settings]] — settings.json deny 规则（本地侧安全红线）

---

## 开发评估：Evals-Driven Development

| 步骤 | 内容 |
|------|------|
| 上线第一天 | 建立 50+ 真实案例的 **Golden Dataset** |
| 每次迭代后 | 自动跑 Regression Set（LLM-as-judge）|
| 验证通过标准 | 技术更新不导致业务逻辑倒退 |

> 关联：[[AI_Team_Coding_Practice]] — 团队侧确定性验证基础设施（vitest + tsc + lint）

---

## 2026 Q2 MVP 快速交付栈

| 维度 | 推荐工具 | 核心价值 |
|------|----------|----------|
| 工作流 | [[AI_Workflow_System|n8n]] / LangGraph | 快速构建复杂逻辑闭环 |
| 观察力 | Langfuse | 生产级监控与 Evals |
| 协议 | MCP | 极速集成企业存量数据 |
| 模型 | Claude Sonnet | 2026 年 Tool-use 最佳性价比 |

---

## 矛盾与争议

- LangGraph vs Claude Code 内置 Subagents：LangGraph 提供更精细的状态机控制（time-travel debug），但引入额外依赖；Claude Code [[Claude_Code_Subagents|Subagents]] 更轻量。两者可混用：Claude Code 负责 inner loop，LangGraph 负责 outer workflow。

---

## 关联实体

- MCP_Production_Agent — MCP 决策树与 context-efficient 模式
- [[Agent_Harness_Engineering]] — 完整 Harness 六层架构与 TAO/ReAct 循环
- Agentic_Memory_System — 记忆层实现（Mem0、Vector Store）
- AI_Team_Coding_Practice — 团队 AGENTS.md/DECISIONS.md 上下文资产体系
- Claude_Code_Subagents — 子代理隔离执行（Claude Code 侧）
- LangGraph_Deep_Agents — LangGraph 状态机运行时详细架构与 Deep Agents 组件包
- [[Solo_Founder_Agent]] — 3 周落地 3 个专业 Agent 的最小可行架构（Research/Content/Operations）
- [[AI_Native_Startup_Playbook]] — Anthropic 官方创业四阶段手册（Idea→MVP→Launch→Scale），Agentic技术债复利与工作流锁定护城河
- [[Context_Engineering]] — 企业级上下文工程的实现原则（Context is State → Write/Select/Compress/Isolate）
- [[Multi_Agent_Architecture]] — Anthropic 参考实现的三层分离（Skills/Agents/MCP）+ 安全分层隔离模式
- [[Enterprise_Agentic_AI_6_Ideas]] — 6 大企业级落地方案（Continuous Audit、Manager Amplification、AI-Native 招聘等，含 Claude Code + MCP + N8N 技术栈）
- [[Enterprise_Agent_Playbook]] — 六个可落地企业用例蓝图（Continuous Audit/Manager Amplification/Builders-Measurers框架/N8N自进化闭环/AI转型咨询）
- [[Bending_Spoons_Universal_OS]] — 分层中央平台架构（Universal OS）：并购后统一基础设施的工程实现

*[Source: raw/Agentic AI公司技术架构.md]*
