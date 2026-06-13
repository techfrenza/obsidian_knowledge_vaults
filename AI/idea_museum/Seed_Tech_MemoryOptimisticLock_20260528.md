---
name: Memory Store Concurrent Write SHA256 Guard
description: Managed Agent Memory 的 SHA256 预条件机制——多 Agent 并发写入同一记忆文件时的乐观锁模式
type: seed
concept: 记忆并发乐观锁（Memory Optimistic Lock）
hook_insight: "你的 5 个并行 Agent 都在更新同一份用户偏好记忆文件——没有锁，最后写入的 Agent 覆盖其他所有人的更新，静默丢失数据。Anthropic 官方解法不是互斥锁，而是 SHA256 预条件：先读哈希，写入时附带预期哈希，不匹配则拒绝——把数据库乐观锁引入了 AI 记忆层"
wiki_link: "[[Managed_Agent_Memory]]"
---

# Memory Store Concurrent Write SHA256 Guard

## 技术核心逻辑

Anthropic Managed Memory Store 的并发写入保护机制：

```python
# 安全更新模式（带 SHA256 预条件）
mem = client.beta.memory_stores.memories.get(store.id, memory_id)

client.beta.memory_stores.memories.update(
    mem.id, memory_store_id=store.id,
    content="新内容",
    precondition={
        "type": "content_sha256",
        "content_sha256": mem.content_sha256  # 读时的哈希
    }
)
# 若写入前内容已被其他 Agent 修改 → 哈希不匹配 → API 拒绝 → 调用方重试
```

这是数据库乐观并发控制（Optimistic Concurrency Control）在 AI 记忆层的实现：
- 不使用互斥锁（避免死锁和性能瓶颈）
- 假设冲突罕见，冲突时拒绝并重试（而非阻塞等待）
- 审计：30 天版本历史，可 `redact` 清除敏感旧版本

## 优缺点对比

优势：
- 防止并发写入的静默数据覆盖（这是分布式系统最危险的 bug 类型）
- 无死锁风险（乐观锁 vs 悲观锁）
- 提供完整的版本历史（时间旅行调试）

劣势：
- 高并发场景下写入冲突率上升，重试逻辑需要工程师自行实现
- 每次写入前必须先 GET（额外一次 API 调用）
- 100KB 文件上限（约 25K tokens）限制了单文件记忆粒度

## 关联
- [[Managed_Agent_Memory]] — 完整 API 使用指南
- [[MultiAgent_Concurrent_Write_Research]] — 多 Agent 并发写入冲突研究
- [[Agentic_Memory_System]] — 自建 Agent 的内存架构对比

[Source: wiki/Managed_Agent_Memory.md]
