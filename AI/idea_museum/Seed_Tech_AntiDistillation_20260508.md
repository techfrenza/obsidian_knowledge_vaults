# Seed: Anti-Distillation Defense

[Concept] 用"有毒的诱饵工具"防止竞争对手训练数据集

[Hook Insight] Anthropic 据称在 API 请求中注入"诱饵工具定义"——看似真实但包含细微错误的虚假工具描述。竞争对手若抓取流量进行模型蒸馏训练，这些"毒药"会悄悄降低其模型质量。这是一种通过"主动污染训练集"来维持竞争优势的防御策略，将安全防御嵌入到数据层而非代码层。

[Wiki Link] [[Unique_Engineering_Insights]] | [[Claude_Code_Security]] | [[Anthropic_Agent_SDK]]
