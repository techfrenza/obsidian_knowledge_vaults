---
type: seed
source: wiki_scan
date: 2026-05-08
---

# 平坦层级的涌现：为什么没有嵌套反而更好

**[Hook]** Anthropic 的 Agent SDK 强制禁止子代理嵌套（flat hierarchy only）。直觉上这像是一个限制。实际上，它比允许嵌套的框架产生更可靠的多智能体协调——因为它消除了上下文污染的级联传播路径。

**[Insight]** 嵌套层级（A 调用 B，B 调用 C）意味着错误可以沿调用链向上传播，每一层都会放大并重新诠释错误信息。平坦层级迫使所有协调通过 Orchestrator 的显式消息传递进行，等效于强制所有跨代理通信"序列化"——这恰好是分布式系统中防止级联故障的标准做法。

**[Counter-intuitive Claim]** 限制嵌套深度 = 提升系统可靠性，不是降低它。"不能嵌套"强迫你把复杂性放在协议层（消息格式、Handoff 规范），而不是调用栈里。

**[Wiki Link]** → [[Anthropic_Agent_SDK]]（平坦子代理层级约束）→ [[Multi_Agent_Architecture]]（三层分离架构）

---
*灵感来源：Anthropic_Agent_SDK 的 flat hierarchy 约束 + Multi_Agent_Architecture 的安全分层隔离设计*
