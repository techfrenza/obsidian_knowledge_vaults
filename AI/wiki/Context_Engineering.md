---
title: Context Engineering（上下文工程）
aliases: ["上下文工程", "Context is State", "Context Primitives"]
tags: [context, engineering, agent, primitives, state]
category: memory-systems
parent: "[[Agent_Engineer_Mental_Models]]"
created: 2026-05-15
date: "2026-05-15"
---

# Context Engineering（上下文工程）

Parent: [[Agent_Engineer_Mental_Models]]
Source: [Source: raw/Building AI agent.md, raw/Agent Engineer - Mental Model.md, raw/How to Become an AI Agent Engineer in 2026 — The Complete Roadmap.md, raw/What to Learn, Build, and Skip in AI Agents.md]

## 核心定义
将上下文视为系统的**实时状态**，而非简单的对话历史。**Context is State。**
上下文是有限且昂贵的资源，目标：用**最少高信号 Token** 驱动模型产生正确行为。

## 四大原语（Context Primitives）
来自 [[MCP_Production_Agent|MCP 协议]]，定义了上下文管理的核心操作：
1. **Write**：scratchpad 和 memory 文件。Agent 将工作状态外化，防止 context 压缩时丢失
2. **Select**：在使用点检索相关信息。不一次性将全部数据倒入 context，只 fetch 当前步骤需要的内容
3. **Compress**：在 context window 85-95% 满时摘要，Auto-compact 旧 turns 再继续，防止 agent 中途耗尽空间
4. **Isolate**：子代理使用独立 context window。将子任务分派给子 agent，返回压缩摘要给父 agent，绝不返回原始数据

**工程核实**：打开任何生产 agent 的 trace 日志，对比第 1 步和第 7 步的 context——数一数有多少 token 还在"挣钱"。第一次做这个检查往往令人尴尬，然后去修，同一 agent 可靠性立刻提升，无需改模型或 prompt。

## 核心技术

### 压缩与剪枝（Compression & Pruning）
- 向量检索（RAG）、语义摘要、基于重要性权重剔除不相关信息
- 解决：长文本模型的"中间迷失（Lost in the Middle）"现象

### 版本化（Versioning）
- 为不同上下文状态建立快照
- 允许 Agent 在任务失败时回滚到有效状态重新尝试

### 分层治理
- **CLAUDE.md**：永久规则层（每次对话自动加载）
- **Subagents**：隔离内存，防止上下文污染，见 [[Claude_Code_Subagents]]
- **Skills**：按需加载的专业知识，仅在语义匹配时激活，见 [[Claude_Code_Skills]]

## 生产工具
| 工具 | 用途 |
|------|------|
| `/compact` | Claude Code 内置压缩命令（四级：微/自动/完全/反应式） |
| RAG | 向量检索替代全量上下文输入 |
| `LangGraph Checkpointer` | 状态持久化，支持中断恢复（见 [[LangGraph_Build_Agents]]） |
| `_hot.md` 缓存 | wiki 层加速重复查询，降低 token 95% |

## Context Rot（上下文腐烂）
长会话中上下文过载导致性能下降、遗忘关键决策。
对策：
- 每 2 周清理 Project 文件，删除 3 周未引用的内容
- 每周 Review Memory，删掉过时内容
- 聊天变乱就新建 chat，让 Project Memory 接管

## 三层上下文框架（Context Layers）

来源：[[Contextmaxxing]] 理论与 "How to Master Context Engineering"。

| 层级 | 内容 | 使用现状 |
|------|------|---------|
| Layer 1：即时上下文 | 当前 Prompt（问题/指令/格式要求） | 99% 的用户仅使用此层 |
| Layer 2：会话上下文 | 上传文件、对话历史、系统指令 | 多数用户部分使用 |
| Layer 3：持久上下文 | 跨会话记忆、偏好文件、知识库 | 绝大多数人未正确使用，最大杠杆点 |

## 四文件上下文架构

每位专业用户推荐维护 4 个持久上下文文件（每份 < 2000 词以适应 context window）：

1. **Identity File**：角色/专长/背景/沟通风格（"对 AI 的入职文档"）
2. **Audience File**：受众画像、知识水平、痛点、用语习惯
3. **Standards File**：质量标准、格式偏好、风格指南、反面示例
4. **Project File**：当前目标、活跃项目、近期决策、开放问题（动态更新）

每次会话开始加载这 4 个文件 → 模型从"通用助手"转变为"了解你工作世界的协作者"。

## 动态上下文加载规则

**反直觉**：将所有知识库塞入每次对话会**降低**性能（注意力稀释）。

正确做法：为每种常见工作类型预定义加载规则：

```
写作任务  → Identity + Audience + Standards + 同格式最佳示例
分析任务  → Identity + Project + 原始数据 + 同主题历史分析
研究任务  → Project + 研究方法论文档 + 现有研究基础
战略任务  → 全部 4 文件 + 竞争格局 + 行业数据
```

## 三种记忆方法（成熟度递进）

| 级别 | 方案 | 适用门槛 |
|------|------|---------|
| 初级 | 手动记忆文档（运行日志） | 立即可用 |
| 中级 | 结构化知识库（Obsidian + Markdown 文件夹） | 20+ 上下文文档时升级 |
| 高级 | 向量数据库 + RAG（PGVector/Chroma + BM25 + RRF） | 无法手动管理时升级 |

Claude Code 可直接从文件系统读取中级知识库中的 Markdown 文件。

## Anthropic 官方：上下文工程定义（2026-06）

来源：[[Effective_Context_Engineering]] — Anthropic Applied AI 团队博客（2026-06-03）

**官方定义**：上下文工程是提示工程的自然演进。提示工程关注如何写好提示词；上下文工程关注 **在每次 LLM 推断时，对整个上下文状态（系统指令/工具/MCP/外部数据/消息历史等）进行最优配置**的策略集合。

**注意力预算（Attention Budget）**：LLM 基于 Transformer 架构，每个 token 与所有其他 token 存在 n² 配对关系。token 数量增加时，注意力被稀释。模型有"注意力预算"，每个新 token 都在消耗它。

**目标公式**：找到能最大化期望结果概率的**最小高信号 token 集合**。

### 长时任务的三种上下文策略

| 策略 | 原理 | 适用场景 |
|------|------|---------|
| **Compaction（压缩）** | 对话接近 context 上限时，让模型总结关键内容重启新窗口；保留架构决策/未解决 bug/实现细节，丢弃冗余 tool 输出 | 需要大量来回的长时对话 |
| **Structured Note-taking（结构化笔记）** | Agent 定期将笔记写入持久化记忆（如 NOTES.md），从窗口外拉回 | 有明确里程碑的迭代开发 |
| **Sub-agent Architectures（子代理）** | 主 Agent 高层规划，子 Agent 深度探索后返回 1000-2000 token 压缩摘要 | 复杂研究/分析中的并行探索 |

### 上下文 Rot（官方确认）

研究（Chroma "context rot"）证明：随 token 数量增加，模型准确召回上下文信息的能力下降。即使百万 token 的窗口也存在这一特性。这不是硬件限制，而是模型结构特性。

### Just-in-Time（JIT）上下文检索

现代 agent 不再预处理所有相关数据上传，而是维护轻量标识符（文件路径/存储查询/链接），在运行时按需加载数据。类比人类认知：不记忆整个语料库，而是有文件系统/收件箱/书签等外部索引系统。

Claude Code 使用混合策略：CLAUDE.md 直接加载，grep/glob/head/tail 按需加载文件。

## 矛盾与争议
压缩 vs 完整性：过激压缩可能丢失关键上下文导致决策错误；过于保守则 token 超限。需根据任务类型调节压缩策略。

Prompt 工程 vs 上下文工程：前者优化 syntax（措辞），后者优化 infrastructure（围绕 prompt 的一切）。两者不互斥，但 infrastructure beats syntax every single time（参见 [[Contextmaxxing]]）。

## 关联概念
- [[Agent_Engineer_Mental_Models]] — 上下文原语是第三大心智模型
- [[Agentic_Loop]] — loop 运行期间的上下文治理
- [[CLAUDE_md_Best_Practices]] — 永久上下文层的最佳实践
- [[Agentic_Memory_System]] — 跨会话记忆与上下文的关系
- [[Tokenmaxxing]] — 不省 Token 策略：把海量 Context 全喂给 Agent 以最大化输出质量
- [[Contextmaxxing]] — 对比视角：最大化 Token 用量 vs 最大化上下文相关性
- [[GBrain_Architecture]] — Garry Tan 的个人知识图谱实现持久化上下文
- [[Agent_Engineer_MOC]] — Agent Engineer 体系学习地图