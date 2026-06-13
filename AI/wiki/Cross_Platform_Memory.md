---
title: Cross-Platform Memory
aliases: ["跨平台记忆优化", "AI Memory Markdown 系统", "Master AI Memory"]
tags: [memory, cross-platform, markdown, obsidian, claude, chatgpt]
category: memory-systems
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Cross-Platform Memory

Parent: [[index]]

> 核心论点：AI 平台各自的原生记忆系统存在孤岛问题。用本地 Markdown 文件作为通用记忆层，可在 Claude / ChatGPT / Grok / Gemini 之间零损失迁移上下文。

---

## Master AI Memory 文件夹结构

```
AI Memory/
├── Identity.md          # 核心身份 + 永久偏好（<300 行）
├── Memory_Writing.md    # 写作 workflow 记忆
├── Memory_Coding.md     # 编码 workflow 记忆
├── Memory_Research.md   # 研究 workflow 记忆
└── Archive/             # 每周备份（AI 无法访问）
```

---

## Identity.md 模板

```markdown
# Identity & Core Preferences

**Who I am:** [角色、背景、目标]

**Output Style:**
- 简洁、无废话、直接实用知识点
- 用中文回复（除非指定英文）
- 代码/命令用 ``` 块
- 列表编号清晰，可直接复制执行

**Hard Rules (NEVER):**
- 不要用 em dashes（——）
- 输出控制在实用列表，不要长篇大论
- 绝不添加背景介绍，直接给可执行步骤

**Preferences:**
- 优先本地 Markdown 文件管理
- 更新后让我确认

**Update Frequency:** 每周 review 一次
```

---

## 使用流程

1. **新聊天/新平台** → 粘贴 `Identity.md` 全文
2. **特定 workflow** → 再粘贴对应 `Memory_X.md`
3. **会话结束** → 输入更新 prompt：
   > "Summarize new preferences, decisions, or workflows discovered. Output Markdown ready to append."
4. 复制输出 → 追加到对应 Memory 文件

---

## 自进化 Loop

会话开头额外加：
> "After pasting Identity + Memory, watch this conversation and at the end suggest updates for Memory.md to make future sessions better."

允许 AI attach 文件夹时，它可自动修改 .md 文件。

---

## Claude 三层记忆体系对比

| 层级 | 设置时间 | 适用场景 |
|------|----------|----------|
| Layer 1：原生 Settings → Memory | 5-10 分钟 | 90% 普通用户 |
| Layer 2：本地 Master Folder（4 个 .md 文件）| ~60 分钟 | 跨聊天持久记忆 |
| Layer 3：Obsidian Vault（[[Claude_Memory_Layers|Karpathy schema]]）| 1-2 小时 | AI 第二大脑，自进化 wiki |

---

## 避坑

- Identity.md 保持 <300 行
- 只记录高价值决策，避免噪声
- 本地存储，隐私安全，可离线访问

---

## 关联实体

- [[Memory_MOC]] — 记忆系统知识地图（全记忆集群索引）
- [[Agentic_Memory_System]] — 技术层面的四类内存实现
- [[Agent_Context_Architecture]] — 企业 Agent 的四层 Context 分层
- [[Managed_Agent_Memory]] — Anthropic API 原生 Memory Store
- [[CLAUDE_md_Best_Practices|CLAUDE.md Best Practices]] — 项目级持久化规则文件最佳实践
- [[Claude_Memory_Layers]] — 三层记忆架构（原生/文件系统/Obsidian Wiki）与跨平台 Markdown 迁移的对接层

*[Source: raw/跨平台记忆优化.md, raw/Claude memories.md]*
