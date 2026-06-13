---
title: Tokenmaxxing（最大化 Token 投入策略）
aliases: ["Token最大化", "烧Token策略", "Boil the Ocean"]
tags: [tokenmaxxing, context, strategy, agent, performance]
category: prompt-engineering
parent: "[[Agent_Engineer_Roadmap]]"
created: 2026-05-15
date: "2026-05-15"
---

# Tokenmaxxing（最大化 Token 投入策略）

Parent: [[Agent_Engineer_Roadmap]]
Source: [Source: raw/Tokenmaxxing.md]

## 核心主张
**烧 Token，不省 Token**。把所有相关 Context（代码、文档、历史 PR、用户反馈、全网数据）一次性塞满，先让 Agent "Boil the Ocean"，再处理细节——输出质量和执行力爆炸式提升。来源：Garry Tan 在 Light Cone 播客中的实践（13 年未写代码 → 靠 Claude Code 在数月内产出数十万行代码）。

## 四步实操框架

### Step 1：搭建 Thin Harness + Fat Skills 基础
| 工具 | 用途 |
|------|------|
| Claude Code | 200K+ 上下文，支持直接读本地文件夹 + bash 工具调用 |
| Claude Projects | 云端知识库，付费版自动开启 RAG |
| OpenClaw / Gstack | 魔改 Agent harness，内置多 Skills |
| `.claudeignore` | 排除 node_modules / logs，防止浪费 Token |

**关键文件**：项目根目录放 `CLAUDE.md`（所有结构化 Skill 指令存放处，见 [[CLAUDE_md_Best_Practices]]）。

### Step 2：把"所有相关 Context"全喂给 Agent
- **代码**：打开整个项目文件夹，Claude Code 自动用 `list_directory`/`read_file`/`grep` 探索。
- **文档 + 历史 PR + 用户反馈**：全部扔进 `docs/` 或 `knowledge/`，在 `CLAUDE.md` 中写："每次开始任务前，先读 knowledge/ 文件夹里的所有 spec、PR-history、user-feedback.md"。
- **全网数据**：通过 Browse Skill（Playwright）+ Perplexity / Tavily 实时抓取，结果注入当前上下文。
- **Context 压缩控制**：在 `CLAUDE.md` 加指令："compact 时只保留架构、关键决策、最新修改文件列表"。

### Step 3：先 "Boil the Ocean"，再聚焦执行
不要上来就写代码——先展开所有可能性：

**Plan-Eng-Review Skill 模板（直接复制到 CLAUDE.md）**
```
1. CEO Plan（Boil the Ocean）：
   - 列出 10-20 种所有可能实现方案（含极端方案/10x 方案）
   - 对每个方案画 ASCII 图（数据流/状态机/架构图）
   - 给出 pros/cons、风险、成本、用户价值
   - 问用户：哪个方向最符合 vision 和 taste？

2. Engineering：
   - 只针对选定方案，生成完整工程方案 + 代码 + 测试
   - 必须先画 ASCII 图，再写代码

3. Review & QA：
   - 自测所有 edge case
   - 用 Codex/Claude 做 code review
   - 输出 PR 格式 + 测试报告
```
触发方式：`/plan-eng-review + 需求`（见 [[Claude_Code_Commands]]）。Human 只做最后 5% 的 taste 判断（见 [[Human_In_The_Loop]]）。

### Step 4：RAG + Hybrid Search 补充外部信息

| 方式 | 实现 |
|------|------|
| 简单版（Claude Projects） | 上传文档，付费版自动开启 RAG + Hybrid Search（语义 + 关键词） |
| 进阶版（Gstack） | PGVector / Chroma 向量库 + BM25 + Reciprocal Rank Fusion (RRF)，工具调用时 top-5 chunks 注入 |

## 完整每日 Workflow（10+ PR/天节奏）
```
/ceo → 10x 思考 + 市场调研
/plan-eng-review → Boil the Ocean + ASCII 图
确认方向 → Agent 生成代码 + 测试
Agent 自 review + Playwright QA
Human 最终 taste 判断 → merge PR
```

## Fat Skills 清单（核心 5 类，见 [[Claude_Code_Skills]]）
- **CEO Skill**：10x 问题清单
- **Designer Skill**：画 ASCII 图
- **DevEx Skill**：最佳实践
- **Browse Skill**：全网搜索
- **QA Skill**：自动测试

## 为什么有效
| 杠杆 | 机制 |
|------|------|
| Tokenmaxxing | 多喂 Context = 买时间和执行力 |
| Boil the Ocean | 先展开所有可能性，防止 Agent 迷路或跑偏 |
| RAG + Hybrid | 外部信息实时补充，质量碾压"省 Token"的孤立 prompt |
| Human 只负责 5% | vision + taste + 最终判断，其余全交 Agent |

## Tokenmaxxing vs Contextmaxxing

| 维度 | Tokenmaxxing | Contextmaxxing |
|------|-------------|----------------|
| 核心策略 | 烧更多 Token = 买更多产出 | 最大化每 Token 的相关上下文质量 |
| 成本瓶颈 | Token 预算上限 | 无关上下文导致的 Token 浪费 |
| 早期验证 | Garry Tan 13 年未写代码靠 Claude Code 产出 10 万行 | Uber 2026 年初耗尽 AI 预算（模型从零重建上下文） |
| 本质 | 第一阶段：证明 AI 有用则大量使用 | 第二阶段：优化 Token 花在哪里 |

两者非对立：Tokenmaxxing 是起点（用量最大化），Contextmaxxing 是进化（质量最大化）。
参见：[[Contextmaxxing]]

## 六大心智模型（"Claude 作为初级同事"）

来源：raw/Claude Code.md 中的 Claude Code 实用心智模型（2026-04-29 实践）

| 心智模型 | 核心操作 |
|---------|---------|
| 把 Claude 当 junior 同事 | 描述现象 + 让 Claude 自己判断原因（而非命令式下达答案） |
| /compact 在 60% Token 时执行 | 注意力重聚焦（不是省 Token）；Claude 问"你想做什么"时立即 compact |
| 跑偏立即 ESC 重开 | 污染 context 后修正成本是重开的 2-3 倍 |
| Sub agents 用于上下文隔离 | 树形而非线性 context；子 agent 失败探索不污染主线 |
| 简单任务丢 Haiku | 1-2 步 → Haiku；5+ 步 → Sonnet/Opus；10+ 步 → ultrathink |
| /loop 用于 monitoring | 异步同事模式：CI 检查/慢查询扫描/PR review 队列 |

## 关联概念
- [[Agent_Engineer_Roadmap]] — Tokenmaxxing 是 Phase 3–4 工程能力的实践形态
- [[Context_Engineering]] — Context 注入策略的理论基础
- [[Contextmaxxing]] — 进阶视角：从最大化 Token 用量到最大化上下文质量
- [[CLAUDE_md_Best_Practices]] — CLAUDE.md 中 Skill 模板的编写规范
- [[Harness_Engineering_Deep_Dive]] — Thin Harness + Fat Skills 架构的实现细节；Plan-Eng-Review 三阶段与 Evaluator-Optimiser 循环同构
- [[AI_OS_Framework]] — Four Cs 框架中 Context 层与 Tokenmaxxing 的对应关系
- [[Prompt_Template_Library]] — Plan-Eng-Review Skill 可从模板库延伸
- [[RLM_Simulation]] — 同一"阶段性 handoff"哲学：Boil the Ocean 后 human 选方向 ↔ peek/partition 后 human 传结果