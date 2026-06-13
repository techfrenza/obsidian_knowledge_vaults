---
type: seed
source: wiki_scan
date: 2026-05-08
---

# AI 系统的测试套件测试的是环境，不是逻辑

**[Hook]** 你为 AI 代理写的"测试"根本不是在测试代码对不对——它在测试你的 harness 对不对。这是一个被大多数工程师误解的范式转变。

**[Insight]** 传统软件测试：输入 → 函数 → 断言输出。AI 系统评估（Evals）：提示词 + 工具 + 上下文 → 模型 → LLM-as-judge 评分 + CI 门禁。核心差异：传统测试验证**确定性逻辑**，Evals 验证**环境配置是否使模型产生正确行为**。同一个模型，不同 harness，成功率从 42% 到 78%（实证数据来自 Unique_Engineering_Insights）。

**[Counter-intuitive Claim]** 写更好的 Evals 不等于测试更多场景——而是测试你的 CLAUDE.md、工具描述、系统提示是否把模型"配置"成了正确的决策者。测试的对象是人写的配置，不是 Anthropic 训练的权重。

**[X Hook Draft]**
> "软件工程师花 80% 时间写测试来验证逻辑。AI 工程师花 80% 时间写 Evals 来验证配置。前者测试代码，后者测试环境。这是两个完全不同的工作——只是名字听起来一样。"

**[Wiki Link]** → [[Unique_Engineering_Insights]]（Harness > 模型实证 78% vs 42%）→ [[AI_Team_Coding_Practice]]（Evals-Driven Development）→ [[Agent_Harness_Engineering]]

---
*灵感来源：Unique_Engineering_Insights 的 Harness > 模型实证 + Enterprise_AI_Architecture 的 Evals-Driven Development 原则*
