---
type: seed
source: wiki_scan
date: 2026-05-05
---

# Seed: 高阶工具悖论 — 封装越多，Agent 越聪明

**[Concept]** MCP 工具粒度越粗（高阶），Agent 执行越可靠；工具越细（低阶），灵活性越高但幻觉率越高。

**[Hook Insight]** 直觉上认为给 Agent 更多细粒度工具 = 更强能力。实际相反：把 `get_thread + parse + create_issue + link` 四个工具合并为 `create_issue_from_thread` 一个高阶工具后，Agent 的错误率显著下降。原因是：低阶工具需要 Agent 自行维护执行序列和中间状态，这是幻觉的温床。高阶工具把"协议知识"嵌入工具定义本身，Agent 只需决策"调不调用"，不需要决策"怎么调用"。Cloudflare 的极端案例：只暴露 `search + execute` 两个工具，让 Agent 自己写脚本覆盖 2500+ endpoints，上下文仅 ~1K tokens。

**[Tech Trade-off]**
- 高阶工具：低幻觉、低 token 消耗、低维护成本 → 缺点：工具爆炸（每个业务流程一个工具）
- 低阶工具：高灵活性、通用性强 → 缺点：Agent 需维护状态链，失败模式难预测

**[Wiki Link]** [[MCP_Production_Agent]] → [[Enterprise_AI_Architecture]]

*[Source: wiki/MCP_Production_Agent.md | 2026-05-05]*
