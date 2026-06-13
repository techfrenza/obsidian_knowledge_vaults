---
title: Managed Agent Memory
aliases: ["Managed Agents Memory", "Memory Store API", "跨 Session 持久学习"]
tags: [managed-memory, anthropic-api, memory-store, persistent, sessions]
category: memory-systems
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Managed Agent Memory

Parent: [[index]]

> 核心论点：Anthropic Managed Agents Memory 是官方 API 级别的跨 Session 持久化方案。Memory 以文本文件形式挂载在 `/mnt/memory/`，Agent 可读写，跨 Session 自动同步。

---

## 启用步骤（5 分钟）

```python
# Step 1: 创建 Memory Store
store = client.beta.memory_stores.create(
    name="User Preferences",
    description="Per-user preferences and project context."
)
# 记录返回的 store.id（格式 memstore_01Hx…）

# Step 2: 可选预填充内容
client.beta.memory_stores.memories.create(
    store.id,
    path="/formatting_standards.md",
    content="All reports use GAAP formatting. Dates are ISO-8601..."
)

# Step 3: 创建 Session 时挂载 Store
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
    resources=[{
        "type": "memory_store",
        "memory_store_id": store.id,
        "access": "read_write",
        "instructions": "User preferences and project context. Check before starting any task."
    }]
)
```

---

## 工作机制

- 所有 Memory 以文本文件存在，挂载路径固定为 `/mnt/memory/`
- 跨 Session 持久化：修改后自动同步，下次 Session 自动加载
- 访问模式：`read_write`（默认）或 `read_only`（防止恶意写入）
- 每个文件上限 **100KB**（约 25K tokens），建议拆成多个小文件
- 单 Session 最多挂载 **8 个** Store

---

## 常用 API 操作

```python
# 列出所有 Memory
page = client.beta.memory_stores.memories.list(
    store.id, path_prefix="/", order_by="path", depth=2
)

# 安全更新（带 SHA256 校验，防并发冲突）
client.beta.memory_stores.memories.update(
    mem.id, memory_store_id=store.id,
    content="新内容",
    precondition={"type": "content_sha256", "content_sha256": mem.content_sha256}
)

# 查看 30 天历史版本
versions = client.beta.memory_stores.memory_versions.list(
    store.id, memory_id=mem.id
)
```

---

## 生产最佳实践

| 场景 | 建议 |
|------|------|
| 共享标准文件 | `read_only`（防止意外覆写）|
| 用户/项目个性化数据 | `read_write` |
| Session 创建前 | 在 `instructions` 明确："挂载的 Memory 必须先检查再行动" |
| 敏感数据清理 | 用 `redact` 清除旧版本 |
| 定期维护 | 每周运行 list + delete 低频文件 |

---

## 与其他记忆方案对比

| 方案 | 适用层 | 持久性 |
|------|--------|--------|
| **Managed Memory Store** | Anthropic API / Managed Agents | 跨 Session，API 级持久 |
| [[Agentic_Memory_System]]（Chroma/pgvector）| 自建 Agent | 自管理向量数据库 |
| [[Cross_Platform_Memory]]（Markdown 文件）| 任意 AI 平台 | 本地文件，手动管理 |
| [[Agent_Context_Architecture]]（working/episodic）| Claude Code 项目 | 项目内持久 |

---

## 快速调用（Claude Code Skill）

- 输入 `/claude-api` 触发 claude-api Skill
- 直接问 "Managed Agents memory 使用方法" → 自动给出最新代码模板

---

## 关联实体

- [[Memory_MOC]] — 记忆系统知识地图（全记忆集群索引）
- [[Agentic_Memory_System]] — 自建 Agent 的内存架构（无需 Anthropic API）
- [[Agent_Context_Architecture]] — 四层 Context 的业务实践
- [[Cross_Platform_Memory]] — 跨平台 Markdown 记忆迁移
- [[Claude_Memory_Layers]] — 三层记忆系统（原生/文件系统/Obsidian Wiki）
- [[Claude_Code_Skills]] — 可用 `/claude-api` Skill 快速调用最新用法

*[Source: raw/Claude Managed Agent memory.md]*
