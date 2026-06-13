---
name: Token 路由按步骤类型而非输入长度
description: 生产 Agent 应根据当前步骤的类型（分类/推理/规划）而非输入 Token 数量来选择模型，可实现 4-8x 成本摊销
type: seed
concept: 按步骤类型路由模型（Route by Step Type, Not Input Length）
hook_insight: 你在输入超过 X 个 Token 时自动升级到 Opus——但 Haiku 处理 100K token 的分类任务比 Opus 处理同样任务便宜 15 倍；真正的路由决策变量不是"多大"，而是"在做什么"
wiki_link: "[[Production_Agent_Engineering]]"
---

# Token 路由按步骤类型而非输入长度

## 技术核心逻辑

Production Agent Engineering 中的 3-Tier 模型路由原则：

| Tier | 用途 | 模型 |
|------|------|------|
| 结构化输出/分类/抽取 | 强制 JSON, schema 匹配 | Haiku（$1/$5 per M） |
| 业务逻辑综合推理 | 80% 的生产工作负载 | Sonnet（$3/$15 per M） |
| 规划/工具选择/歧义消解 | >5 tool calls, 意图不明 | Opus（$15/$75 per M） |

核心规则：**Route by step type, not input length**。

## 优缺点对比

**优势**
- 典型生产工作负载实测：4–8x 成本摊销
- 分类步骤即使输入 100K token 也可用 Haiku，因为任务本身不需要推理深度
- 与 [[GBrain_Fat_Thin_Architecture]] 中 Skill 级模型选择权互补：每个 Skill 自己决定用哪个模型

**劣势 / 陷阱**
- 需要精确定义每类步骤的模型边界，否则退化为按 token 数的启发式路由
- 步骤类型判断本身可能需要一个 Haiku 级分类器——引入递归成本
- 跨 Provider 路由（Claude + GPT + DeepSeek）增加 fallback 复杂度

[Source: wiki/Production_Agent_Engineering]
