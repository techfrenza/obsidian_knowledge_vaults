---
title: Claude Memory Layers（三层记忆系统）
aliases: ["三层记忆", "Claude Memory", "持久记忆架构"]
tags: [memory, claude, layers, cross-session, knowledge-base]
category: memory-systems
parent: "[[Agentic_Memory_System]]"
created: 2026-05-15
date: "2026-05-15"
---

# Claude Memory Layers（三层记忆系统）

Parent: [[Agentic_Memory_System]]

> 构建跨 session 持久记忆的完整三层架构，从 5 分钟设置到自进化知识库。[Source: raw/Claude memories.md]

---

## Layer 1：Claude 原生 Memory（5 分钟设置）

- **位置**：Claude Settings → Memory
- **操作**：
  - 检查并清理过时的自动保存项
  - 手动添加核心角色、工作风格、永久偏好
  - 对话中直接输入 `"Remember: [具体指令]"` 或 `"Forget that I mentioned [x]"`
- **Projects 模式**：为每个 workflow 创建独立 Project，在 Project Instructions 粘贴完整上下文

---

## Layer 2：桌面文件系统（60 分钟初始设置）

```
Claude Master Folder/
├── Instructions.md    ← Who you are / Rules / Good outputs look like
├── Memory.md          ← ## Preferences / ## Corrections / ## Patterns / ## Decisions
├── Context.md         ← 当前项目具体上下文（可按项目拆分子文件夹）
└── Archive/           ← 每周手动备份整个 Master Folder（Claude 无法访问）
```

- 在 Claude Cowork 或 Claude Code 中 Attach 整个 Master Folder
- Claude 自动读写 `.md` 文件，实现跨聊天持久记忆
- 遇到新规则直接说，Claude 自动更新 `Memory.md`

---

## Layer 3：AI Second Brain（自进化知识库）

### 简单版（Notion）
- Claude Settings → Connectors 启用 Notion
- 创建专用 "Memory Database" 页面
- 工作时输入 `"Send this to my Notion Memory Database"` → Claude 自动写入

### 高级版（Obsidian Wiki）
- [[AI_OS_Framework|Karpathy]] LLM Wiki 架构：`/raw` → `/wiki`（概念/实体/来源）
- 每次新 source 自动更新 10-15 个页面
- 与 [[AI_OS_Framework]] 的 Wiki 层完全对应

---

## 跨 session 持久化对比

| 层级 | 持久性 | 设置成本 | 适用场景 |
|------|--------|----------|----------|
| Layer 1 | 跨 session，但受 Claude 控制 | 5 分钟 | 偏好/风格记忆 |
| Layer 2 | 完全自控，本地文件 | 60 分钟 | 项目上下文/决策日志 |
| Layer 3 | 自进化，结构化知识图谱 | 数小时 | 知识资产长期积累 |

---

## 日常维护

- Cowork 模式 Attach Master Folder 比普通 Chat 消耗更多 tokens，但记忆准确度大幅提升
- 每周备份 Archive 文件夹，防止意外覆写
- Layer 2 手动编辑 `.md` 后可直接 Attach 到任何新聊天立即生效

---

## 相关链接

- [[Memory_MOC]] — 记忆系统知识地图（全记忆集群索引）
- [[Agentic_Memory_System]] — 四类内存架构（In-context/External/Episodic/Parametric）
- [[Managed_Agent_Memory]] — Anthropic 官方 Memory Store API
- [[Cross_Platform_Memory]] — 跨 AI 平台 Markdown 迁移
- [[AI_OS_Framework]] — Four Cs 框架中的 Context 层