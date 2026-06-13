---
concept: Validation Contract First
hook: 为什么"先写测试"还不够——真正的 Agent 纪律是在写任何代码前就锁死"什么叫 Done"
wiki_link: [[Multi_Agent_Missions_System]]
tags: [seed, anti-intuitive, multi-agent]
---

**Concept**: Validation Contract（验证契约）先于代码

**Hook Insight**: 大多数人的"测试驱动开发"是写完代码后补测试，或者至少在同一 session 里。Factory Missions 的反直觉做法：**Validation Contract 在规划阶段提前写好**，包含数百条独立于实现的断言，强制在写第一行代码前就定义"什么叫完成"。这不是 TDD，这是"Contract-Driven Execution"。结果是验证阶段占掉大部分 wall clock time——但这恰恰是长期任务能跑 16 天不漂移的原因。

**X Hook Draft**: 你的 AI 为什么总是"差不多完成了"但就是差那么一点？因为你让它自己定义"完成"。Factory 的解法：在写第一行代码前，强制输出 Validation Contract（几百条独立断言）。模型完成后必须证明每条断言被覆盖，不是"看起来差不多"。这就是 Agent 能跑 16 天不漂移的原因。
