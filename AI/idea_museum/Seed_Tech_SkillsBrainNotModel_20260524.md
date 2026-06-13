---
concept: 模型是引擎，Skill + Brain 才是车
hook: 为什么花大量时间调模型 prompt 是在抱错大腿——真正的杠杆在架构
wiki_link: [[GBrain_Fat_Thin_Architecture]]
tags: [seed, anti-intuitive, gbrain, fat-skills]
---

**Concept**: Fat Skills + Thin Harness = AI 真正的杠杆点

**Hook Insight**: 大多数人把时间花在优化 prompt 上（调模型），但 GBrain 架构的反直觉之处是：真正的复利在 Fat Skills 和 Fat Data，不在模型本身。Thin Harness 几千行代码，Fat Skills 100+ 个 Markdown 文件，每个只做一件事。Garry Tan 凌晨 2 点编码的不是 prompt，而是这套架构。模型可以随时替换（Opus 做 precision、GPT-5.5 做 recall、DeepSeek 做 creative），因为 Skill 是模型无关的。

**X Hook Draft**: Garry Tan 的 AI 系统里有 100,000 页知识库、100+ 个 Skills、15 个 crons，但核心 Harness 只有几千行代码。反直觉的是：复利不在模型，在架构。模型是引擎，可以随时换。Skills + Brain 才是难以复制的护城河。你花多少时间调 prompt，又花多少时间建 Skills？
