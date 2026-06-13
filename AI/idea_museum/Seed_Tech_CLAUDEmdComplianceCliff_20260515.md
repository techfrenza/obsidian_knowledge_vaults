---
name: CLAUDEmdComplianceCliff
description: CLAUDE.md 存在合规性悬崖——超过 200 行后 Claude 遵守度急剧下降，4000 tokens 后降至 30%；更多规则 ≠ 更强约束
type: seed
concept: CLAUDE.md 信息密度上限
hook_insight: "你花了 3 小时写了 300 行规则的 CLAUDE.md，结果 Claude 的合规率反而比 65 行版本低 70%——规则文档有硬上限，超过它就是在给遗忘提供更多靶点"
wiki_link: "[[CLAUDE_md_Best_Practices]]"
---

## 技术核心逻辑

Karpathy 12 规则系统的实测数据揭示了一个反直觉的合规曲线：
- **<65 行**：高合规率（Karpathy 目标区间）
- **>200 行**：急剧下降（规则丢失现象）
- **>4000 tokens**：合规率降至 ~30%

原因：CLAUDE.md 在每个请求时全量加载进上下文。规则越多，注意力越分散，每条规则的"信噪比"越低。这与直觉相反——人类写合规文档时，覆盖越全越好；但 LLM 的注意力是有限资源，更多内容 = 更多被忽略。

## 权衡

| 多规则 | 少规则 |
|--------|--------|
| 感觉覆盖更全 | 实际合规率更高 |
| 难以维护（哪条重要？） | 每条规则权重高 |
| 容易产生规则冲突 | 强制优先级思考 |

**设计原则**：CLAUDE.md 是竞争资源，不是备忘录。每新增一条规则，就是在稀释所有已有规则。

*[Source: wiki/CLAUDE_md_Best_Practices.md]*
