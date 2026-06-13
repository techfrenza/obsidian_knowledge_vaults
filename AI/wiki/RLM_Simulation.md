---
title: RLM Simulation
aliases: ["递归语言模型", "Recursive Language Model", "长文档处理", "Context Rot 防治"]
tags: [rlm, context-rot, long-context, recursive, partition]
category: prompt-engineering
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# RLM Simulation

Parent: [[index]]

> 核心论点：通过 peek / grep / partition / recurse 四个工具，手动模拟 RLM（Recursive Language Model）处理超长上下文任务，每个 prompt 上下文保持极短，彻底消除 Context Rot。

---

## 核心思路

把长文档当"外部变量"（ctx），主模型只处理当前子任务，通过结构化对话递归拆解。

---

## 四个虚拟工具

| 工具 | 参数 | 用途 |
|------|------|------|
| `peek(ctx, n=2000)` | ctx=文档名, n=字符数 | 查看前 n 个字符，了解结构 |
| `grep(ctx, pattern)` | ctx=文档名, pattern=正则 | 过滤相关行/段落 |
| `partition(ctx, k=5)` | ctx=文档名, k=份数 | 平均分成 k 份，返回起始位置和摘要 |
| `recurse(subtask)` | subtask=子任务描述 | 对子任务发起递归调用 |

---

## System Prompt（复制粘贴）

```
你现在是 RLM（Recursive Language Model）。你的目标是处理超长上下文任务，
永远不要一次性把整个文档塞进 prompt。

可用工具（必须严格按格式使用）：
- peek(ctx, n=2000)
- grep(ctx, pattern)
- partition(ctx, k=5)
- recurse(subtask)

规则：
- 每次只输出一个工具调用或最终答案
- 工具调用格式：Tool: peek | grep | partition | recurse
  Args: {"ctx": "文档名", "n": 2000}
- 永远保持思考简洁，只关注当前子任务
```

---

## 完整流程示例（5000 条客服票据）

```
用户：从 customer_tickets_5000.txt 找出 user123 所有 billing 相关票据并总结原因

Step 1 → Tool: peek / Args: {"ctx": "customer_tickets_5000.txt", "n": 2000}
Step 2（用户粘贴结果）→ 了解结构
Step 3 → Tool: grep / Args: {"ctx": "...", "pattern": "user123"}
Step 4（用户粘贴结果）→ 获取相关票据
Step 5 → Tool: partition / Args: {"ctx": "user123_tickets.txt", "k": 10}
Step 6 → Tool: recurse / Args: {"subtask": "分析 partition_3 中的 billing 问题原因"}
```

---

## 实用技巧

- **Claude 更适合**：Projects 可长期保持文档 + 更好遵循结构化指令
- **GPT-4o 优势**：支持更长的单次输出，可一次处理多个子任务
- **加速方法**：提前让模型 partition 整个文档（第一步就做计划）
- **并行处理**：开 3-5 个并行对话，每个处理一个 partition

---

## 常见错误避免

- 不要让模型直接"总结整个文档"
- 每次只给当前需要的子内容
- 最终答案前要求列出所有递归路径（增加可解释性）

---

## 关联实体

- [[Agent_Context_Architecture]] — Context Rot 是 RLM 解决的核心问题
- [[Agent_Harness_Engineering]] — Compaction 和 JIT retrieval 是类似思路的 Harness 实现
- [[Agentic_Memory_System]] — In-context 滑动窗口防溢出策略
- [[Context_Engineering]] — RLM 的 peek/grep/partition/recurse 四工具是 Compress + Select 原语的手动实现版本
- [[Tokenmaxxing]] — 同一"阶段性 handoff"哲学：Boil the Ocean 后 human 选方向 ↔ peek/partition 后 human 传结果
- [[Contextmaxxing]] — RLM 的定期预编译摘要与 Contextmaxxing 的"预编译知识 > 查询时重建"是同一理念的不同粒度实现
- [[LangGraph_Deep_Agents]] — 长程任务状态机：RLM 是无框架 Context 管理，LangGraph 是结构化状态图版本

*[Source: raw/模拟RLM.md]*
