---
type: seed
source: wiki_scan
date: 2026-05-02
concept: "越晚 Fork Subagent，缓存优势越大"
wiki_link: "[[Claude_Code_Subagents]]"
---

# [Hook Insight] You're Using AI Wrong If You Fork Early

大多数人第一直觉是：任务开始就派生子代理，让它并行探索。

**反直觉**：你应该先在主线程深度理解 codebase，积累大量 prompt cache，**然后**再 Fork。

因为 Fork 继承的不只是上下文，而是已经付过费的 prompt cache token。fork 的那一刻，后续所有子 Agent 输入 token 便宜 10 倍。你等得越晚，fork 的单位价值越高。

早 fork = 用高价 token 探索  
晚 fork = 用低价 cache 分裂已有理解

这意味着 AI 工作流的经济学和人类团队协作的经济学完全相反：人类团队越早并行越省钱，AI 越晚并行越省钱。

*[Wiki: [[Claude_Code_Subagents]]]*
