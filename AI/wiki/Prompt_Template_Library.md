---
title: Prompt Template Library
aliases: ["Prompt 模板库", "40 Expert Prompts", "专家级提示词"]
tags: [prompts, templates, writing, analysis, strategy, communication]
category: prompt-engineering
parent: "[[Prompt_Engineering_Library]]"
created: 2026-05-08
date: "2026-05-08"
---

# Prompt Template Library

Parent: [[Prompt_Engineering_Library]]
Source: [Source: raw/40 prompts.md, raw/40个专家级prompts.md]

> 核心论点：专家级 prompt 的本质是**结构化角色 + 约束规则 + 输出格式**的三元组。替换 `[变量]` 即可跨模型（Claude/GPT/Gemini）达到一致的专家级输出。

---

## 40 个 Prompt 分类总览

| 类别 | 编号 | 核心用途 |
|------|------|----------|
| 写作与内容 | 01-10 | 文章、线程、邮件、文案、故事 |
| 分析与战略 | 11-20 | SWOT、决策矩阵、市场机会、OKR |
| 技术与开发 | 21-28 | 架构、代码审查、调试、API 设计 |
| 个人生产力 | 29-32 | 周计划、学习、谈判、习惯 |
| 数据与研究 | 33-35 | 数据解读、调查分析、研究综合 |
| 沟通 | 36-40 | 困难对话、反馈、演讲、道歉、电梯演讲 |

---

## 高频核心模板（直接复制）

### The Expert Article Writer（01）
```
你是为顶级出版物写作的高级内容策略师。
写一篇 [字数] 关于 [主题] 的文章。
受众：[描述]，角度：[独特视角]
结构：Hook → 问题 → 框架 → 证据 → 行动
规则：段落 ≤3 句，无废话，每节最重要的句子加粗。
```

### The Decision Matrix（12）
```
在 [选项列表] 中做决策。背景：[情况描述]
建立矩阵：5 个标准（权重合计 100%），给每个选项打分。
输出：2 段推荐理由 + 承认次优选项 + 会改变决定的条件。
```

### The Architecture Advisor（21）
```
构建 [系统]。需求：[列表]，规模：[X]，预算：[Y]
提出 2 种方案：架构图、技术选型、优缺点、复杂度、#1 风险。
推荐其中一种 + 前 5 个执行步骤。
```

### The Code Reviewer（22）
```
审查以下代码：[代码]
检查：安全性、逻辑、性能、可读性、最佳实践。
每个问题输出：严重程度、位置、原因、修复（展示修改后代码）。
```

### The Root Cause Analyzer（13）
```
问题：[描述]。
5 Why 技术逐层挖掘，区分症状与根因。
提出 3 个解决方案（表面/中层/根本）。推荐其中一个。
```

---

## 使用工作流

1. 按类别找到对应 prompt（编号 01-40）
2. 替换 `[变量]`，输入具体上下文
3. 每周挑 5 个最相关 prompt 使用，保存自定义版本
4. 一个月后形成个人模板库

---

## 与其他笔记的联系

- [[Prompt_Engineering_Library]] — 完整提示词工程体系
- [[Research_Prompts]] — 研究专向 prompt 集合
- [[Claude_Code_Skills]] — 将 prompt 封装为可复用 skill
- [[Context_Engineering]] — 高质量 prompt 的上下文前置条件
- [[Claude_Optimization]] — 模型输出质量优化策略

---

## 矛盾与争议

- 结构化 prompt 的边际递减：过度套模板会抑制模型发散思维，适合执行任务，不适合创意探索。
- 跨模型一致性：模板在 Claude 上表现最优，GPT/Gemini 的 CoT 触发机制略有差异。
