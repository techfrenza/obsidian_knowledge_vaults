---
title: Prompt Engineering Library
aliases: ["专家级Prompts", "40个专家级prompts", "Prompt Templates", "Expert Prompts"]
tags: [prompts, prompt-engineering, templates, writing, analysis, technical, productivity]
category: prompt-engineering
parent: "[[index]]"
created: 2026-05-02
date: "2026-05-02"
---

# Prompt Engineering Library

Parent: [[index]]

> 核心论点：结构化 Prompt 模板比自由式提问效率高 3-5 倍。关键在于**角色定义 + 规则约束 + 输出格式**三要素缺一不可。按类别封装后，直接替换 `[]` 变量即可复用。

---

## 分类总览

| 类别 | 编号 | 核心用途 |
|------|------|----------|
| Writing & Content | 01-10 | 文章、线程、邮件、内容复用、标题生成 |
| Analysis & Strategy | 11-20 | SWOT、决策矩阵、市场扫描、OKR、风险评估 |
| Technical & Dev | 21-28 | 架构设计、代码审查、Debug、API/DB 设计、测试 |
| Productivity & Personal | 29-32 | 周规划、学习加速、谈判、习惯设计 |
| Data & Research | 33-35 | 数据解读、调研综合 |
| Communication | 36-40 | 困难对话、反馈、演示、道歉、Elevator Pitch |

---

## 模板结构共性（必须包含）

所有高效 Prompt 模板遵循三要素：

```
角色定义:  "You are a [expert role] who [concrete credential]."
结构约束:  Structure: [step1, step2, step3...]
规则限制:  Rules: [negative constraints, format rules]
```

---

## Writing & Content (01-10)

### #1 Expert Article Writer
```
You are a senior content strategist who has written for top-tier publications.
Write a [WORD COUNT]-word article about [TOPIC].
Audience: [WHO THEY ARE and WHAT THEY KNOW]
Angle: [YOUR UNIQUE TAKE]
Structure: Hook, Problem, Framework, Evidence, Action.
Rules: Paragraphs ≤3 sentences, no filler phrases, no hedge words, bold the most important sentence in each section.
```

### #2 Thread Architect (X/Twitter)
```
Write a Twitter/X thread about [TOPIC].
Structure: Tweet 1 hook, Tweets 2-3 problem, Tweets 4-10 framework, Tweet 11-12 example, final tweet takeaway + CTA.
Rules: Each tweet <280 characters, no hashtags, no emojis unless meaningful, each tweet stands alone and flows.
```

### #3 Email Drafter
```
Draft an email for: [DESCRIBE THE SITUATION, THE RECIPIENT, AND YOUR GOAL]
Tone: [professional/casual/direct/diplomatic]
Rules: Subject line action-oriented, opening in first sentence, max 3 short paragraphs, clear next step.
Generate 2 versions with different tones.
```

### #4 Content Repurposer
```
Take this content and repurpose it into 5 formats:
Twitter thread (12 tweets), LinkedIn post (200-300 words), Newsletter intro,
3 standalone social posts, Short-form video script (60 seconds).
Rules: Each format native to platform, maintain core argument.
```

### #8 Headline Generator
```
Generate 20 headline options for: [BRIEF DESCRIPTION]
Categories: 5 curiosity, 5 benefit, 5 contrarian, 5 specific-number.
For each: rate predicted click-through 1-10 and explain. Rank top 3.
```

---

## Analysis & Strategy (11-20)

### #11 SWOT Analyzer
```
Perform comprehensive SWOT of [COMPANY/PRODUCT].
For each quadrant: 5 specific items with WHY. Rate impact High/Medium/Low.
End with: #1 strategic priority, biggest risk if ignored, first action this week.
```

### #12 Decision Matrix
```
Decide between [OPTIONS]. Context: [BACKGROUND].
Build matrix: 5 criteria (weight 100%), score each option.
Write 2-paragraph recommendation + acknowledge runner-up + condition that would change it.
```

### #13 Root Cause Analyzer
```
Problem: [DESCRIBE].
5 Whys technique, identify symptoms vs root.
Propose 3 solutions (surface/mid/root). Recommend one.
```

### #18 OKR Builder
```
Create OKRs for [TEAM/PERIOD]. Context: [SITUATION].
For each Objective: 3-4 Key Results with baseline/target/measure + confidence. Flag conflicts.
```

### #19 Risk Assessor
```
About to [INITIATIVE].
7 most likely risks: Probability/Impact, warning sign, mitigation, contingency.
Plot 2x2 matrix, top 3 to monitor.
```

---

## Technical & Dev (21-28)

### #21 Architecture Advisor
```
Build [SYSTEM]. Requirements: [LIST], scale, budget.
Propose 2 approaches: diagram, tech choices, pros/cons, complexity, #1 risk.
Recommend one + first 5 steps.
```

### #22 Code Reviewer
```
Review this code: [CODE]
Check: Security, Logic, Performance, Readability, Best Practices.
For each issue: Severity, location, why, fix (show corrected code).
```

### #23 Debug Diagnostician
```
Error: [MESSAGE + STACK]. Context: [WHAT CODE DOES].
Explain meaning, 3 likely root causes, evidence for each, fix, prevention.
```

### #24 API Designer
```
Design REST API for [SYSTEM].
For each endpoint: method/path, request/response schema, auth, rate limit.
Include error format, pagination, versioning, 3 security concerns.
```

### #26 Test Case Generator
```
Test cases for [FUNCTION/FEATURE].
Categories: Happy Path (3), Edge Cases (5), Error Cases (3), Security (2), Performance (1).
For each: name, input, expected output, why matters.
```

---

## Productivity & Personal (29-32)

### #29 Weekly Planner
```
Goals for quarter: [LIST], last week: [SUMMARY], commitments: [LIST].
TOP 3 Priorities, SCHEDULED, BUFFER TASKS, DELIBERATELY SKIPPING (most important).
```

### #30 Learning Accelerator
```
Learn [TOPIC/SKILL]. Level: [BEGINNER etc], Time: [HOURS/WEEK], Style: [PRACTICAL], Goal: [WHAT TO DO AFTER].
Prerequisites, Core concepts, Projects, Resources, Milestones, Timeline.
```

---

## Communication (36-40)

### #38 Presentation Outliner
```
Presentation for [TOPIC]. Audience, duration, goal.
Structure: Opening, Problem, Solution, Evidence, Objection handling, CTA.
For each section: slide content, notes, transition.
```

### #40 Elevator Pitch Builder
```
Pitch for [IDEA]. Audience, context, time.
Hook, Problem, Solution, Proof, Ask.
3 versions (bold, conversational, data-driven).
```

---

## 使用方法

1. 直接复制对应 Prompt，替换 `[]` 变量
2. 每周挑 5 个最相关 Prompt 使用并保存自定义版本
3. 一个月后形成个人模板库

与 [[Claude_Code_Skills]] 的 Karpathy Loop 结合：对高频 Prompt 建立自动评估流水线，持续提升输出质量。

---

## 关联实体

- [[Claude_Code_Skills]] — Skill 中可封装这些 Prompt 模板，通过 description 字段触发特定模板
- [[CLAUDE_md_Best_Practices]] — CLAUDE.md 中可嵌入 Writing/Review 类 Prompt 约束写作风格
- [[AI_Team_Coding_Practice]] — Technical 类 Prompt (#22 Code Reviewer, #26 Test Case Generator) 直接支持 Evals-Driven Development
- [[Agent_Harness_Engineering]] — Generator/Reviewer Prompt 模板可嵌入 Harness 的 Skill Layer
- [[AI_Orchestration_System]] — Analysis Prompt (#11 SWOT, #19 Risk Assessor) 适配 Agent 决策框架
- [[Research_Prompts]] — 学术/论文场景的专项 4 步提示词工作流（论点提取→敌对审稿→Steelman→24h 提升）
- [[Unique_Engineering_Insights]] — 提示词设计的底层哲学（System Prompt 是"宪法"而非"台词"）
- [[Prompt_Engineering_Advanced]] — 进阶方法：Metaprompting 将模板迭代进化成"超级提示"，Prompt Folding 实现动态分类路由
- [[Metaprompting]] — 元提示四步循环：v1 普通 Prompt → 超级提示迭代优化；Bookmarkability Rubric 量化 X 内容质量
- [[Prompt_Template_Library]] — 40 个即用 Prompt 模板完整列表（写作/分析/技术/沟通，含 `[变量]` 替换模板）
- [[Prompt_Engineering_MOC]] — 提示工程知识地图（全系 5 页统一索引）

*[Source: raw/40个专家级prompts.md]*
