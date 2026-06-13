---
name: MultiAgent Context Race Condition
description: 多Agent并行时代码用git保护，但AI上下文资产（CLAUDE.md/DECISIONS.md）完全裸奔——工程师有并发意识，但只保护了错误的东西
type: seed
concept: AI上下文资产的并发盲区
hook_insight: "你的代码库有 git merge 保护，但 5 个并行 Agent 同时写入 DECISIONS.md 时，谁的决策最终生效？没人知道——因为这个问题从来没有被提出过"
wiki_link: "[[MultiAgent_Concurrent_Write_Research]]"
---

## 技术核心逻辑

多Agent并行编排已是标准实践（[[AI_Orchestration_System]] Night Queue + 5-10 并行 Session），但工程师的并发防护意识仅覆盖代码文件：

```
已有保护：
├── 代码文件     → git merge / rebase（成熟机制）
└── 数据库写入   → 事务/锁（标准基础设施）

完全裸奔：
├── CLAUDE.md    → 全局规则文件，无锁，last-write-wins
├── DECISIONS.md → 架构决策日志，并发追加 = 顺序随机
└── Memory Store → Obsidian wiki，无并发协议
```

**根本矛盾**：AI 上下文资产是 Harness 的神经系统（[[Harness_Engineering_Deep_Dive]]），但被当作静态文档对待，没有数据库基本的并发保证。

## 优缺点对比

| 方向 | 优势 | 代价 |
|------|------|------|
| 单一写入者（Master Orchestrator） | 零冲突，简单 | 并行化效益打折，Orchestrator 成为瓶颈 |
| 乐观锁 + 版本戳 | 保留并行性 | 冲突时需人工仲裁，增加 HITL 负担 |
| 分区隔离（各 Agent 私有上下文） | 高并行度 | 合并逻辑复杂，知识碎片化 |
| CRDT（无冲突复制） | 自动合并 | CLAUDE.md 语义约束规则不适合 CRDT 合并 |

**核心洞察**：这不是技术难题，而是认知盲区。工程师在"AI 生成上下文"之前已形成"上下文 = 只读配置"的心智模型，而 Agentic 时代的上下文是可写的、竞争的资产。

*[Source: wiki/MultiAgent_Concurrent_Write_Research.md]*
