---
name: Recitation（背诵）不是冗余 Token，是注意力管理的工程原语
description: 在长 Agentic Loop 中，要求 Agent 每轮循环开始时重述整体目标和当前计划，是对抗"目标漂移"的唯一确定性机制，而非提示词啰嗦。
type: seed
concept: Recitation-as-Attention-Anchor（背诵即注意力锚定）
hook_insight: 你在 Agent Prompt 里加了"每轮循环开始时重述当前目标"，同事说这是浪费 Token。实际上 Anthropic Harness Engineering 把这条列为"上下文治理"的三大机制之一——和 Subagent 隔离、自动压缩并列。原因：在 10+ 步的 Agentic Loop 中，模型的注意力会被工具输出的噪声逐渐淹没，导致"目标漂移"——它还在执行，但执行的不再是你要的目标。Recitation 不是提示词的礼貌格式，它是每轮循环的语义复位操作。
wiki_link: "[[Harness_Engineering_Deep_Dive]]"
---

## 技术核心逻辑

Harness Engineering 的上下文治理三大机制对比：

| 机制 | 作用 | 实现成本 |
|------|------|----------|
| Subagents 隔离 | 防止上下文污染 | 高（架构改动） |
| Automatic Compaction | 防止上下文溢出 | 中（工具配置） |
| Recitation（背诵） | 防止目标漂移 | 极低（一句 Prompt） |

**目标漂移的机制**：
- 工具调用返回的内容（文件内容、API 响应、错误日志）占据注意力
- 随着循环深入，最初的目标逐渐被"当前状态信息"稀释
- 模型开始优化"当前工具返回的问题"而非"原始任务目标"

**Recitation 的正确写法**：
```
每次循环开始时，输出以下内容（不超过2行）：
- 总体目标：[原始任务目标]
- 当前计划步骤：[当前处于哪一步，下一步是什么]
然后继续执行。
```

## 优缺点对比

**优势**：
- 实现成本极低：一句 Prompt 约 30 个 Token
- 防止"目标幻觉"：Agent 不会在深层循环中忘记最初要做什么
- 可审计性：每轮的 Recitation 输出形成天然的执行日志

**劣势**：
- 每轮额外消耗约 50-100 Token（成本极低但非零）
- 若 Recitation 写得过长，本身会成为注意力竞争者而非锚定器
- 不能替代真正的 Compaction：Recitation 防漂移，Compaction 防溢出

## 关联
- [[Harness_Engineering_Deep_Dive]] — Recitation 在 Harness 五大机制中的位置
- [[Context_Engineering]] — 上下文治理四大原语
- [[Agentic_Loop]] — 长循环中目标漂移的根本机制
- [[CLAUDE_md_Best_Practices]] — 系统级注意力管理

[Source: raw/Claude Code Harness Engineering 指南.md]
