---
title: MultiAgent Concurrent Write Research
aliases: ["多 Agent 并发写入", "并发上下文冲突", "Multi-Agent Memory Conflict"]
tags: [research-gap, multi-agent, concurrency, memory, context-engineering]
category: agent-engineering
parent: "[[Multi_Agent_Architecture]]"
created: 2026-05-25
date: "2026-05-25"
status: "待研究"
---

# 多 Agent 并发写入冲突 — 待研究课题

Parent: [[Multi_Agent_Architecture]]

> **知识缺口**：当多个并行 Agent Session 同时读写同一套上下文资产（CLAUDE.md / DECISIONS.md / Memory Store）时，冲突解决策略尚无系统性覆盖。

---

## 问题的三种来源（独立发现，同一空白）

此知识缺口被三个独立的综合报告分别标记，说明这是跨场景的普遍问题：

| 来源报告 | 具体表述 |
|---------|---------|
| `claude-code-memory-control-synthesis.md` | "多个并行 Session/Worktree 同时写入同一 Memory Store，冲突解决策略未定义" |
| `memory-context-architecture-synthesis.md` | "当多个子 Agent 并行执行，如何设计跨 Agent 记忆合并与冲突解决协议" |
| `ai-orchestration-os-synthesis.md` | "5-10 个并行会话共享 AGENTS.md/DECISIONS.md，并发写入冲突如何处理，哪个 Agent 决策有优先权" |

---

## 涉及的上下文资产类型

```
写入竞争场景：
├── CLAUDE.md         (规则文件，全局共享)
├── DECISIONS.md      (架构决策日志，追加写入)
├── Memory Store      (Obsidian wiki / .md 文件网络)
└── Project files     (代码、配置，通过 git 管理)
```

git 管理的文件（代码）已有成熟冲突解决机制（merge / rebase）；
**真正缺失的是非 git 管理的 AI 上下文资产的并发协议。**

---

## 候选解决方向

### 方向 A：CRDT（无冲突复制数据类型）
- 适用：DECISIONS.md 追加日志（天然 append-only，接近 CRDT G-Set 语义）
- 局限：CLAUDE.md 的规则是有序且相互约束的，CRDT 合并可能产生语义矛盾

### 方向 B：乐观锁 + 事件溯源
- 参考：[[AI_Orchestration_System]] 提出"File-system-as-State + 乐观锁"思路
- 适用：共享状态文件加版本戳，写入前检测版本，冲突时触发人工仲裁

### 方向 C：单一写入者架构（Master Agent）
- 参考：[[Enterprise_Agent_Playbook]] 的 Orchestrator Agent 作为协调者
- 所有 Worker Agent 只读上下文资产；写入权限仅归 Orchestrator

### 方向 D：分区隔离（各 Agent 独立 Context 文件）
- 参考：[[Claude_Code_Subagents]] 的上下文隔离原则
- Agent 各自维护私有 DECISIONS-{agent-id}.md，定期由 Orchestrator 合并

---

## 相关参考
- [[Multi_Agent_Architecture]]：多 Agent 协调模式总览
- [[Agent_Harness_Engineering]]：Harness 作为协调层的设计
- [[Claude_Code_Subagents]]：上下文隔离 vs Fork 继承的权衡
- [[SAP_Agent_Memory_Service]]：企业级 Memory Service 是否有相关策略
- [[Harness_Over_Model_Principle]]：Harness 层设计公理

---

## 矛盾与争议

此问题尚无任何笔记给出系统性答案。当前知识库中最接近的实践是 git worktree 隔离（[[Claude_Code_Advanced_Features]]），但其保护的是代码文件而非 AI 上下文资产。
