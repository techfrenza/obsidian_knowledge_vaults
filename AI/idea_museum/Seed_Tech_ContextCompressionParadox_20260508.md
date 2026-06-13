---
type: seed
source: wiki_scan
date: 2026-05-08
---

# 上下文压缩悖论：可靠性与完整性的不可能三角

**[Concept]** Context_Engineering 的四大原语（Write/Select/Compress/Isolate）中，Compress 是最危险的。压缩必然丢失信息；问题是：**你不知道被丢弃的信息是否关键**，直到模型做出错误决策。

**[Trade-off Core]**
- **过度压缩**：上下文精简，token 消耗低，但关键约束可能被摘要掉，导致模型在边缘情况下决策错误
- **欠压缩**：保留完整性，但触碰 token 上限，模型进入 Context Rot 状态（注意力稀释，末尾信息权重下降）
- **"正确"压缩率**：任务类型依赖——代码生成任务对精确度敏感（宁可截断不能摘要），推理任务可接受有损摘要

**[Non-obvious Insight]** 压缩决策本质上是一个**二阶不确定性问题**：你不知道模型不知道什么。这意味着压缩策略无法被通用优化，只能针对具体任务类型手动调参。`CLAUDE.md` 的存在是对此问题的务实解法——用永远置顶的规则文件对抗上下文压缩对核心约束的侵蚀。

**[Pros]** Isolate（子 Agent 隔离）可以绕过此悖论：每个子任务有独立完整上下文，无需在单一 context 内压缩。

**[Cons]** Isolate 引入协调开销，且无法在子任务间传递隐式上下文（只能传递显式序列化信息）。

**[Wiki Link]** → [[Context_Engineering]]（矛盾与争议：压缩 vs 完整性）→ [[Claude_Code_Subagents]]（Isolate 模式实现）→ [[Agent_Harness_Engineering]]（Context Collapse 恢复层级）
