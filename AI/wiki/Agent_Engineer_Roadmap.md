---
title: Agent Engineer Roadmap 2026
aliases: ["Agent Engineer", "AI Agent 学习路径", "Agent 2026路线图"]
tags: [agent-engineer, roadmap, langgraph, harness, learning-path]
category: agent-engineer
parent: "[[Agent_Harness_Engineering]]"
created: 2026-05-15
date: "2026-05-15"
---

# Agent Engineer Roadmap (2026)

Parent: [[Agent_Harness_Engineering]]
Source: [Source: raw/Agent Engineer.md, raw/Building AI agent.md, raw/What to Learn, Build, and Skip in AI Agents.md, raw/How to Become an AI Agent Engineer in 2026 — The Complete Roadmap.md]

## 核心定位
Agent Engineer 的真实工作：构建、harness 并运营 agent systems，而非拼接框架角色。
- 同一模型（Opus 4.5）在不同 harness 下性能差距达 78% vs 42%。**Harness 是决定性因素，不是模型本身。**（实证详见 [[Unique_Engineering_Insights]]）
- 核心问题："Framework tourism"——学一堆框架但无法落地。

## 两大核心栈
1. **[[LangGraph_Deep_Agents]]** — 生产默认编排层
2. **[[Anthropic_Agent_SDK]]** — 参考 harness，理解模型如何驱动工具

其余框架（AutoGen、CrewAI 等）已边缘化，跳过。

## 6 阶段学习路径

| 阶段 | 时长 | 核心任务 | 输出物 |
|------|------|----------|--------|
| Phase 0 | 1–2 周 | 建立 mental models（workflow vs agent、augmented LLM、[[Context_Engineering]]） | 2 页个人文档 |
| Phase 1 | 2–3 周 | 从 scratch 写 100 行 loop → Claude Agent SDK 重构 | daily-briefing agent |
| Phase 2 | 3–4 周 | LangGraph + Deep Agents（parallel sub-agents、PostgresSaver、HITL） | LangSmith trace |
| Phase 3 | 3–4 周 | 自建 1500 行 mini-harness（loop、tools、compression、hooks、OTEL） | post-mortem vs Claude SDK |
| Phase 4 | 3–4 周 | golden dataset、trajectory evals、LLM-as-judge、CI gating（见 [[AI_Team_Coding_Practice]]）| GitHub Actions PR block |
| Phase 5 | 持续 | cost discipline、latency、sandboxing（Modal/E2B）、prompt caching | production-ready agent |

## 核心技能点
- [[Context_Engineering]]：Write/Select/Compress/Isolate 四大原语
- [[Agent_Harness_Engineering]]：loop、tool dispatch、context 管理
- [[Claude_Code_Subagents]]：sub-agents 隔离，防止 token 爆炸
- Evals + CI gates：golden dataset → LLM-as-judge → GitHub Actions block
- [[Claude_Code_Security|Sandboxing]]：Modal/E2B 生产隔离

## 5 过滤测试（Framework Launch Filter）

每次新框架/工具发布时，先跑这 5 问，再决定是否投入学习：

| 测试 | 问题 | 通过标准 |
|------|------|---------|
| 1. 持久性测试 | 两年后还重要吗？ | Primitive（协议/模式/沙盒方案）> Wrapper |
| 2. 生产验证 | 有靠谱团队写了诚实的 postmortem？ | 找"我们试了 X，这是踩的坑"，而非 launch 公告 |
| 3. 破坏性 | 采用它需要丢弃 tracing/auth/config？ | 好工具槽入现有系统，不强制迁移 |
| 4. 跳过成本 | 跳过 6 个月的代价是什么？ | 多数情况答案是"零" |
| 5. 可测量性 | 能否测量它对 agent 的实际帮助？ | 无法量化 = 依赖直觉，高风险 |

**操作建议**：新框架发布时，写下"6 个月后我需要看到什么才相信它重要"，再来 check 答案。大多数框架不需要你今天评估。

---

## Q3 2026 观察清单

需持续关注、但尚未明确的信号项：

- **Replit Agent 4 并行分叉模型** — 首次真正解决多 agent 共享状态问题；若稳定，orchestrator-subagent 默认模式可能转变
- **结果导向定价成熟度** — Sierra/Harvey 收入验证窄垂直领域可行；是否泛化到通用场景仍是开放问题
- **Skills 作为打包标准** — AGENTS.md/skills 目录在 GitHub 的扩散；是否标准化如 MCP 对工具的作用待观察
- **开源模型追上差距** — DeepSeek-V3.2 native thinking-into-tool-use + Qwen 3.6；闭源默认优势不永久，每季度 re-eval
- **语音成为默认支撑层** — Sierra 语音频道 2025 年底超越文本；若跨垂直成立，延迟/实时工具调用成一阶问题

---


- **博客**：Anthropic Engineering Blog、Hamel Husain、Eugene Yan、Lilian Weng、Simon Willison
- **课程**：DeepLearning.AI（LangGraph + Agentic AI）、LangChain Academy、Anthropic Interactive Prompt Engineering
- **开源**：Anthropic Cookbook、deepagents、inspect_evals

## 矛盾与争议
无直接矛盾，但"一个周末建 $1B 公司"属于营销语言；实际路径需 17 周扎实执行。

## 延伸
- [[Tokenmaxxing]] — Phase 3–4 的实践形态：不省 Token，靠 Boil the Ocean + RAG 实现 400x 产出
- [[Hermes_Agent]] — 路径之外的移动端层：自进化 Agent，用于 24/7 定时任务和口袋端触达