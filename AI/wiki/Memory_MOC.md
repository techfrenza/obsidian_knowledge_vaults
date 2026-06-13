---
title: Memory MOC（记忆系统知识地图）
aliases: ["记忆系统地图", "Memory Index", "AI Memory MOC"]
tags: [moc, memory, agent, cross-session, index]
category: moc
parent: "[[index]]"
created: 2026-05-16
date: "2026-05-16"
---

# Memory MOC（记忆系统知识地图）

Parent: [[index]]

> 记忆是 Agent 系统的脊柱——无记忆的 Agent 每次都从零开始，无法积累经验或维持身份。本 MOC 覆盖从理论架构到跨平台实现的完整记忆知识链。

---

## 记忆层级总览

```
理论框架
  └─ Agentic_Memory_System     ← 四类内存架构（In-context / External / Episodic / Semantic）

实现层（Claude 生态）
  ├─ Claude_Memory_Layers      ← 三层记忆（原生 Memory / CLAUDE.md / External KB）
  └─ Managed_Agent_Memory      ← Anthropic API 级持久化（/mnt/memory/ 文件挂载）

跨平台层
  └─ Cross_Platform_Memory     ← Markdown 通用记忆层（Claude/GPT/Gemini 零损失迁移）
```

---

## 节点地图

| 笔记 | 层级 | 核心概念 |
|---|---|---|
| [[Agentic_Memory_System]] | 理论 | 四类内存分区（In-context / Vector DB / Episodic / Semantic）；三大职能 Continuity/Context/Learning |
| [[Knowledge_Graph_Memory]] | 进阶实现 | Pydantic Schema控制的知识图谱：多跳推理/本体设计/10-10-10约束 |
| [[Claude_Memory_Layers]] | 实现 | 三层架构；5 分钟设置 Claude 原生 Memory；自进化知识库 |
| [[Managed_Agent_Memory]] | 实现 | Anthropic 官方 Memory Store API；`/mnt/memory/` 跨 Session 同步 |
| [[Cross_Platform_Memory]] | 跨平台 | Markdown Master AI Memory；Claude/ChatGPT/Grok/Gemini 共用记忆层 |

---

## 关键决策树

```
需要跨 Session 持久化？
  ├─ 是 → 用 Anthropic API？
  │         ├─ 是 → Managed_Agent_Memory（官方 API 方案）
  │         └─ 否 → Claude_Memory_Layers（本地文件方案）
  └─ 否 → 仅当前 Session → Agentic_Memory_System（In-context 窗口管理）

需要跨平台（多 AI 工具）？
  └─ Cross_Platform_Memory（Markdown 通用层）

需要多跳推理 / 结构化属性查询？
  └─ Knowledge_Graph_Memory（Pydantic Schema + Zep/Graphiti 知识图谱）
```

---

## 延伸阅读
- [[Contextmaxxing]] — Context 最大化策略（与记忆系统互补）
- [[Claude_Code_Self_Evolving]] — Corrections→Rules 闭环（记忆的自进化实现）
- [[Hermes_Agent]] — 每 15 次工具调用自动写入 skill 文件（轻量级记忆积累模式）
- [[RLM_Simulation]] — In-context 滑动窗口防溢出（记忆压缩技术）
- [[MultiAgent_Concurrent_Write_Research]] — 多 Agent 记忆竞争（并发写入冲突是记忆系统核心未解问题）
