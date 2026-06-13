---
title: Agentic Memory System
aliases: ["AI Agent 记忆层", "四类内存架构", "Vector Memory"]
tags: [memory, agent, episodic, vector-db, chromadb, persistent]
category: memory-systems
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Agentic Memory System

Parent: [[index]]

> 核心论点：Agentic Memory 的三大职能是 **Continuity**（身份/偏好持久）、**Context**（任务链维护）、**Learning**（规避历史错误）。四类内存分工明确，缺一不可。

---

## 四类内存分区

### 1. In-context（窗口内）
- 系统 prompt + 会话历史 + tool results + scratchpad
- 防溢出三策略：
  - **Summarization**：每 10 轮压缩旧历史为 <200 token 摘要
  - **Selective retention**：保留 fact/decision/tool result，丢弃闲聊
  - **Offload**：重要事实提取到 External Vector Store，JIT 拉取

### 2. External（外部存储）
- **Structured Store**（PostgreSQL / Redis / SQLite）：精确 key/ID 查询
- **Vector Store**（Chroma / pgvector / Pinecone）：语义相似度检索

### 3. Episodic（事件日志）
- 每任务结束记录：`{task, action, outcome, pain_score, timestamp}`
- 存入 JSONL + embed 到 Vector Store
- 新任务时 retrieve 最相似 episode 作为 few-shot 参考

### 4. Parametric（模型权重）
- 仅作为通用世界知识 fallback
- 时间敏感/私密内容**绝不依赖**

---

## 完整 Memory Flow（每次请求）

```
Retrieve（continuity + context + relevant episodic）
→ LLM call（带所有内存）
→ Execute tools
→ Write back（更新 episodic + external）
```

---

## 代码模板（直接用）

**Chroma 本地检索：**
```python
import chromadb
client = chromadb.PersistentClient(path=".memory/vector")
collection = client.get_or_create_collection("episodic")
results = collection.query(query_texts=[user_query], n_results=5)
```

**Episodic 日志：**
```python
def log_episode(task, action, outcome, pain_score):
    episode = {"task": task, "action": action, "outcome": outcome,
               "pain_score": pain_score, "timestamp": now()}
    collection.add(documents=[json.dumps(episode)], ids=[str(uuid())])
```

---

## 向量数据库选型

| 场景 | 推荐 |
|------|------|
| 本地开发/小项目 | ChromaDB（零配置）|
| 已用 Postgres | pgvector（零额外 infra）|
| 生产大规模 | Pinecone / Qdrant |

---

## 遗忘策略（防噪声膨胀）

1. **Time-based decay**：recency × semantic_relevance 打分，自动衰减旧记忆
2. **Importance scoring**：写时让 LLM 打分，只存 >7 分项
3. **Periodic consolidation**：夜间 job 合并相似记忆为单条 canonical summary

```
cron: 0 3 * * * python .memory/consolidate.py
```

---

## Memory as Architecture: Retrieval Design Is 80% of the Work

> "Good memory architecture is 20% storage and 80% retrieval design. If you don't retrieve the right memories, the agent behaves as if they don't exist."

**Memory flow pattern**: memory operations bookend every LLM call — retrieval before, write-back after. The model is stateless; the memory system creates the illusion of a stateful, aware agent.

### Memory Management Strategies

**Time-based decay** (from Generative Agents paper, Park et al. 2023):
```python
score = relevance * 0.4 + importance * 0.3 + recency_score * 0.3
# recency = decay_factor^hours_old (decay_factor ≈ 0.995)
```

**Importance scoring at write time**: ask the model to rate output importance (0.0–1.0) before storing. Only persist high-scoring items. Filters noise at the source.

**Periodic consolidation**: nightly job merges near-duplicate memories (cosine similarity > 0.92) into canonical summaries. Analogous to human sleep memory consolidation.

### Memory + Skills as the Same World Model

Skills and memory are not separate systems — they are two faces of the **world model**:
- **Memory**: records what happened, observes the world
- **Skills (SKILL.md)**: codifies observation into actionable procedure ("world has responded to X,Y,Z by producing T")

Memory observes; skills codify. Both improve by reading each other. Systems like Cognee store skills and memory in the same graph store. A skill change emits memory events; memory improvements amend the skills attached. (Source: raw/Memory isn't a plugin. Skills aren't a plugin. They're the same harness.md)

> "The harness that wins treats memory and skills as one comprehensive world model from the start."

## 关联实体

- [[Knowledge_Graph_Memory]] — Schema-controlled graph memory for multi-hop reasoning (Pydantic ontology pattern)
- [[Memory_MOC]] — 记忆系统知识地图（全记忆集群索引）
- [[Agent_Context_Architecture]] — 四层分区的业务视角（Episodic / Semantic / Procedural / Working）
- [[Managed_Agent_Memory]] — Anthropic 官方 Managed Memory Store API
- [[Cross_Platform_Memory]] — 用 Markdown 文件跨 AI 平台迁移记忆
- [[Agent_Harness_Engineering]] — Memory 在 Harness 中的集成位置
- [[Context_Engineering]] — Context is State 原则与四大原语（Write/Select/Compress/Isolate）
- [[LangGraph_Build_Agents]] — LangGraph 记忆分层（Episodic/Semantic/Procedural）的运行时实现
- [[Agentic_Loop]] — 记忆在 loop 各阶段（执行→观察→反思）的读写时机
- [[Contextmaxxing]] — 记忆作为经济基础设施：有记忆的 Agent 从已知状态出发，Token 用于推理而非重建
- [[GBrain_Architecture]] — GBrain 的 Compiled Truth + Append-only Timeline = External 记忆 + Episodic 日志的结构化落地
- [[Multi_Agent_Architecture]] — Dreaming 机制是 Episodic→Procedural 转化的生产级自动化实现（Harvey 6x 完成率）

*[Source: raw/Agentic Memory.md]*
