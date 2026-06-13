---
name: Seed_Tech_DynamicContextLoadingAntiPattern
description: 上下文加载量与 AI 输出质量呈倒 U 型关系：加载过少不够用，加载过多反而注意力稀释
type: seed
concept: Dynamic Context Loading Anti-Pattern
hook_insight: "把所有相关文档都给 AI"是一个反模式——真正的专家工作流是：外科医生只看当前患者的术前影像，不是整个医院的病历库
wiki_link: "[[Context_Engineering]]"
---

## 技术核心逻辑

大多数人对"给 AI 更多上下文"的直觉是正确的——在 context 不足时。但存在一个临界点：

**Context 不足** → 模型缺乏必要信息 → 输出通用化、不精准
**Context 适量** → 模型有足够且相关的信号 → 输出精准、专业化
**Context 过载** → "Lost in the Middle"效应 → 注意力稀释 → 输出质量下降，模型开始忽略中间段内容

## 四类任务的最优加载规则

```
写作任务  → Identity + Audience + Standards + 同格式最佳示例
           （不需要：技术文档、历史代码）
分析任务  → Identity + Project + 原始数据 + 历史分析
           （不需要：受众偏好、通用标准）
研究任务  → Project + 研究方法论 + 现有研究基础
           （不需要：Identity、受众）
战略任务  → 全部 4 文件 + 竞争格局 + 行业数据
```

## 权衡对比

| 加载策略 | Token 成本 | 注意力聚焦度 | 输出精准度 |
|---------|-----------|------------|---------|
| 全量加载（所有文档） | 极高 | 低 | 中等（注意力稀释）|
| 无策略加载（每次重新想） | 低但不一致 | 中 | 低（不稳定）|
| 动态规则加载（本方案） | 中 | 高 | 高 |
| RAG 自动检索 | 中 | 中（依赖检索质量） | 中高 |

## 工程启示

需要为每种工作类型**提前写好加载规则**，而不是在每次会话时临时决定。这些规则本身存储在 CLAUDE.md 或 Project 文件中，形成"元上下文"（决定上下文的上下文）。

参见：[[CLAUDE_md_Best_Practices]] 的会话规则层；[[Contextmaxxing]] 的信号密度指标；[[Agentic_Memory_System]] 的 JIT 拉取策略。
