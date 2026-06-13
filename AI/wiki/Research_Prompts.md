---
title: Research Prompts（研究写作提示词库）
aliases: ["研究提示词", "Research Writing Prompts", "敌对审稿人提示"]
tags: [prompts, research, writing, templates, academic]
category: prompt-engineering
parent: "[[Prompt_Engineering_Library]]"
created: 2026-05-15
date: "2026-05-15"
---

# Research Prompts（研究写作提示词库）

Parent: [[Prompt_Engineering_Library]]
Source: [Source: raw/Research prompts.md]

## 核心工作流（4 步顺序执行）

### Step 1：提取核心论点 + 原创性评估
```
You are a research methodology expert. Here are my raw notes: [粘贴原始笔记/数据]。
Identify the 3 strongest arguments buried in this data, rank them by originality,
and show me exactly where each one challenges or extends existing literature.
```

### Step 2：模拟敌对审稿人 + 识别有效反对意见
```
Now simulate a hostile peer reviewer with a PhD in this field. Generate every
serious objection they would raise against my thesis. Then tell me which
objections actually have merit and which ones I can dismantle.
```

### Step 3：强化最弱论点（Steelman）
```
Take my weakest argument and steelman it harder than I did. Show me what it
would look like if it were airtight. Then tell me what I'd need to prove
to get it there.
```

### Step 4：24 小时最高提升建议（最强收尾 Prompt）
```
You are my thesis advisor. I have 24 hours before submission. Read this draft
[粘贴当前论文草稿] and tell me the single change that would move this from
a B+ to an A. Be brutal.
```

### 整合 Prompt（Step 5）
```
Rebuild my entire research paper step by step. Start from raw notes →
strongest arguments → address objections → steelman weak points →
final high-impact conclusion. Use the previous outputs.
```

## 使用规则
- 按顺序执行：Step 1 → 2 → 3 → 4 → 5
- 每步输出保存为中间文档，供下一步使用
- 遇到卡点时，直接把当前草稿贴入 Step 4（"single change"建议）

## 适用范围
- 学术论文提交前质量提升
- 研究报告/技术博客的论证强化
- 任何需要"敌对测试"的文档（投资备忘录、产品 PRD、架构设计文档）

## 关联概念
- [[Prompt_Engineering_Library]] — 完整提示词库
- [[AI_Orchestration_Practice]] — Plan-First 执行流程（类似的多步骤工作流）
- [[Claude_Optimization]] — XML Tags 与结构化 Prompt 的最佳实践
- [[Prompt_Template_Library]] — 通用提示词模板库（Research_Prompts 是其研究写作专项子集）