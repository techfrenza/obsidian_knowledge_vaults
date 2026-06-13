---
name: RLM Recursive Partition Pattern
description: 手动模拟递归语言模型行为以处理超长上下文——把文档当外部变量而非上下文内容
type: seed
concept: 文档即外部变量（Document as External Variable）
hook_insight: "你把 10 万字文档塞进 context window 希望 AI 理解全文——正确做法是永远不塞进去。把文档当数据库，用 peek/grep/partition/recurse 四个虚拟工具让 AI 外科式取数，上下文始终保持极短"
wiki_link: "[[RLM_Simulation]]"
---

# RLM Recursive Partition Pattern

## 技术核心逻辑

RLM（Recursive Language Model）模拟的本质是将"处理文档"与"理解文档"解耦：

- **传统思路**：把全文塞入 context，期待 AI 记住所有细节
- **RLM 思路**：文档 = 外部变量（ctx），主 Agent 只持有当前子任务所需片段

四个虚拟工具实现精准取数：
```
peek(ctx, n=2000)    → 先探结构
grep(ctx, pattern)   → 精准过滤
partition(ctx, k=5)  → 分片处理
recurse(subtask)     → 递归子任务
```

## 优缺点对比

优势：
- Context Rot 彻底消除：每个 prompt 只含当前相关内容
- 可并行化：3-5 个对话同时处理不同 partition，速度提升 3-5x
- Token 质量最大化：无噪音填充，Signal Density = 1

劣势：
- 需要人工传递工具调用结果（目前为手动循环）
- 无状态：跨 partition 的跨引用逻辑需要额外的综合步骤
- 学习曲线：工程师需要放弃"一次性提问"的心理模型

## 关联
- [[RLM_Simulation]] — 四工具完整 System Prompt
- [[Contextmaxxing]] — 同一理念：预编译 > 查询时重建
- [[Agent_Context_Architecture]] — Context Rot 的系统性解法

[Source: wiki/RLM_Simulation.md]
