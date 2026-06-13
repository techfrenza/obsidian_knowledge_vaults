---
type: seed
source: wiki_scan
date: 2026-05-06
---

# Seed: Subagent 综合陷阱 — 隔离了上下文，却外包了理解

**[Concept]** Subagent 解决上下文污染问题，但引入了一个更隐蔽的风险：主线程 Agent 在不理解结果的情况下继续执行

**核心技术逻辑**

[[Claude_Code_Subagents]] 的核心价值是上下文隔离——子代理在独立窗口处理高噪声任务，主线程只接收 ≤2000 token 的精炼摘要。

**但 wiki 里有一条规则被放在不显眼处**：
> "当子代理回报结果时，主代理必须先读懂并亲自写出后续指令，**严禁将理解工作二次外包**。"

这意味着：

```
危险模式：
  主 Agent → 子 Agent A → 子 Agent B → 子 Agent C
                                         ↓
                              主 Agent 接收摘要，继续执行
                              （主 Agent 实际没理解发生了什么）

正确模式：
  主 Agent → 子 Agent A → [主 Agent 读结果，自己写下一步] → 子 Agent B
```

**权衡核心**：Subagent 隔离的是"上下文"，但无法隔离"理解责任"。如果主线程 Agent 把"理解子代理输出"也外包出去，整个系统就失去了可审计性——没有任何一个 Agent 真正理解了全貌。

**Why it matters**：深度嵌套的 Agent 链是幻觉传播的温床。每增加一层 delegation，错误累积的概率乘以子代理数量。

**[Wiki Link]** [[Claude_Code_Subagents]] — 日常工作流 + 子代理输出约束  
**[Wiki Link]** [[Agent_Harness_Engineering]] — 强制合成（Synthesize）原则
