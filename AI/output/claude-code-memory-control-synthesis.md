---
date: 2026-05-25
source_notes:
  - "[[Agentic_Memory_System]]"
  - "[[Anthropic_Agent_SDK]]"
  - "[[Claude code CLI -running 2 skills in background and front]]"
  - "[[Claude_Code_Advanced_Features]]"
  - "[[Claude Code Commands Reference]]"
tags:
  - synthesis
  - memory-architecture
  - agent-sdk
  - parallel-execution
  - context-control
  - claude-code
---

# Claude Code 记忆-控制-并行 — 跨笔记综合

## 综合单元

> 核心笔记：[[Agentic_Memory_System]]、[[Anthropic_Agent_SDK]]、[[Claude code CLI -running 2 skills in background and front]]、[[Claude_Code_Advanced_Features]]、[[Claude Code Commands Reference]]
> 邻居笔记：[[Memory_MOC]]、[[Claude_Memory_Layers]]、[[Managed_Agent_Memory]]、[[Context_Engineering]]、[[Agentic_Loop]]、[[Agent_Harness_Engineering]]、[[Claude_Code_Subagents]]、[[Claude_Code_Hooks]]、[[Claude_Code_Skills]]、[[CLAUDE_md_Best_Practices]]、[[Claude_Code_Settings]]、[[Claude_Code_MOC]]、[[Human_In_The_Loop]]、[[MCP_Production_Agent]]、[[Opus_4_7_Migration]]

---

## 一致主线

跨越这五篇笔记的统一论断：**Claude Code 的核心工程问题是上下文窗口的稀缺性管理**——记忆架构（四层分区 + 遗忘策略）、代理循环设计（子代理上下文隔离）、并行执行模式（Fork/Worktree 防文件冲突）和命令体系（/compact、/clear、subagent 委托）全部指向同一个目标：**用最少的高信号 Token 驱动最大的推理产出**。

无论是 Agentic Memory System 的 Episodic 日志 + 向量检索、Anthropic SDK 的子代理上下文隔离、前后台并行的 `context: fork` 声明、Advanced Features 中的 `/compact` 与分层 CLAUDE.md、还是 Commands 里的"60% 触发压缩"原则，所有组件都是这一同一稀缺资源战略的不同战术表现。

---

## 内在张力

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Memory 以持久化优先：Episodic 日志写回 + Managed Memory Store 跨 Session 同步，强调记忆累积 | [[Agentic_Memory_System]]、[[Managed_Agent_Memory]] | Context Rot 原则：每 2 周清理 Project 文件，老化内容必须删除，记忆膨胀是性能毒药 | [[Context_Engineering]]、[[CLAUDE_md_Best_Practices]] |
| 子代理应继承父会话完整上下文（Fork 继承 + prompt cache，子 agent 输入 Token 便宜 10 倍） | [[Claude_Code_Subagents]] | 子代理应上下文隔离（Subagents 解决的核心问题是上下文污染，主线程只接收精炼摘要 ≤2000 token） | [[Claude_Code_Subagents]]、[[Anthropic_Agent_SDK]] |
| 并行执行能力（多 Session + Worktrees + /batch）代表效率最大化 | [[Claude code CLI -running 2 skills in background and front]] | 并行执行带来的成本与 Token 失控风险需要 Task Budgets + 熔断机制对冲 | [[Claude Code Commands Reference]]、[[Agentic_Loop]] |
| CLAUDE.md 是规则的权威来源，每天更新 2 次保持新鲜 | [[Claude_Code_Advanced_Features]] | CLAUDE.md 必须严控在 60-80 行，超过 150 条 Claude 会丢规则 | [[CLAUDE_md_Best_Practices]] |

---

## 涌现洞察

**"控制平面与数据平面的分离"是 Claude Code 架构的隐含设计哲学**：只有把所有笔记放在一起才能看到这个模式——Hooks 是确定性执行层（控制平面），Memory/Context 是动态信息载体（数据平面），两者通过明确的接口（PreToolUse/PostToolUse + CLAUDE.md 层级加载）保持解耦。这个模式在任何单一笔记中都没有被明确命名，但它解释了为什么"Hooks 做不需要 LLM 推理的确定性操作，Skills 做语义理解后的流程，CLAUDE.md 持久规则，Memory 持久状态"这四者之间不存在功能重叠——它们分别属于控制平面的不同层次，而非功能的重复。

---

## 知识缺口

**尚未回答的关键问题**：当 Agent 在多个并行 Session/Worktree 中同时写入同一个 Memory Store（如 Managed Agent Memory 的 `/mnt/memory/` 文件）时，冲突解决策略是什么？`Managed_Agent_Memory` 提到了 SHA256 校验的安全更新 API，但没有定义"谁赢"的策略（last-write-wins？乐观锁？版本向量？），而 `Claude code CLI 并行` 的 Worktree 机制只解决文件系统冲突，不解决 Memory Store 并发写问题。

**下一步探索建议**：研究分布式记忆系统中 CRDT（无冲突复制数据类型）在 Agent Memory 中的应用——多个并行 Agent 各自维护的 Episodic 记录最终能否通过合并策略而非串行写入实现无冲突的集体学习。

---

## 参考笔记路径

- `wiki/Agentic_Memory_System.md`
- `wiki/Anthropic_Agent_SDK.md`
- `wiki/Claude code CLI -running 2 skills in background and front.md`
- `wiki/Claude_Code_Advanced_Features.md`
- `wiki/Claude_Code_Commands.md`
- `wiki/Memory_MOC.md`
- `wiki/Context_Engineering.md`
- `wiki/Claude_Code_Subagents.md`
- `wiki/Claude_Code_Hooks.md`
- `wiki/CLAUDE_md_Best_Practices.md`
- `wiki/Agent_Harness_Engineering.md`
