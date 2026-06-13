---
type: seed
source: wiki_scan
date: 2026-05-02
concept: "DECISIONS.md 的复利机制与记忆折旧曲线"
wiki_link: "[[AI_Team_Coding_Practice]]"
---

# [Concept] DECISIONS.md Is Not Documentation — It's Compounding Memory Capital

团队 AI 编码实践的核心主张：每次任务结束的 Compound 步骤（写回 DECISIONS.md）才是长期价值来源，而非当次代码生成速度。

## 权衡核心

| 行为 | 短期成本 | 长期收益 |
|------|---------|---------|
| 跳过 Compound 步骤 | 省 5 分钟 | 下次任务重新发现同一失败模式（-30 分钟）|
| 写回 DECISIONS.md | 多 5 分钟 | AI 下次读取后永久规避该错误路径 |
| 更新 AGENTS.md | 多 3 分钟 | 所有后续 Agent 自动继承约束，团队知识不随人员流动 |

## 非直觉点

DECISIONS.md 的价值不在于记录"做了什么"，而在于记录"为什么不做另一个选项"。  
被拒绝的方案（rejected alternatives）是 AI 最难从代码本身推断的信息——代码只展示最终选择，不展示背后的权衡。这正是 AI 在下次任务中最容易重蹈覆辙的地方。

**折旧曲线**：记忆的价值随时间衰减，但 DECISIONS.md 是抗衰减的——每次新任务重新激活它，它的价值反而随 codebase 复杂度增长而增长。

*[Source: [[AI_Team_Coding_Practice]], [[Agent_Context_Architecture]]]*
