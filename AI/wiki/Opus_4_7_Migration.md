---
title: Opus 4.7 Migration Guide
aliases: ["Claude Opus 4.7", "4.7 迁移指南", "adaptive thinking", "xhigh effort"]
tags: [opus-4-7, migration, effort-level, adaptive-thinking, tokenizer]
category: prompt-engineering
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Opus 4.7 Migration Guide

Parent: [[index]]

> 核心论点：Opus 4.7 引入三个关键变化：新 tokenizer（token 数增加 1.0-1.35x）、`xhigh` effort 级别、`adaptive` thinking 替代 `budget_tokens`。迁移重点是"字面执行"而非"猜测意图"。

---

## 三大关键变化

| 变化 | 影响 | 应对 |
|------|------|------|
| 新 tokenizer | 相同输入 token 数增 1.0-1.35x | 批量测试旧 prompt，观察是否字面执行 |
| `xhigh` effort 级别 | 介于 high 和 max 之间 | 编码/代理任务首选；`max` 只用于极难子问题 |
| `adaptive` thinking | 替代 `budget_tokens` | 所有 `budget_tokens` 代码必须替换，否则 400 错误 |

---

## 立即执行的 API 变更

```python
# 旧（会报 400 错误）
thinking: {"type": "enabled", "budget_tokens": 10000}

# 新（必须改为）
thinking: {"type": "adaptive"}
# effort 参数控制深度
```

---

## Effort Level 策略

```bash
# CLI 用法
claude --model claude-opus-4-7 --effort xhigh "你的任务"

# 同一对话动态切换
"先用 xhigh 思考架构，再切换 high 执行代码"
```

| Effort | 适用场景 |
|--------|----------|
| `high` | 日常编码、文档 |
| `xhigh` | 编码和代理任务（**默认首选**）|
| `max` | 极难子问题（成本最高）|

---

## Prompt 优化技巧（4.7 特有）

### 批量提问，停止多轮澄清
```
"同时回答以下 3 个问题：1. … 2. … 3. …
每个答案独立分段，用编号标记。"
```

### 用正面示例代替"不要"规则
超过 3 条 `don't/never` → 翻转为正面示例：
```
"像这样输出：
示例1: [理想输出]
示例2: [理想输出]
严格按此格式。"
```

### 删除旧的进度脚手架提示
4.7 原生自动输出高质量进度，删除：
- "每 3 次 tool call 总结一次"
- "先解释计划再执行"
- "给我状态更新"

### 明确要求 fan out / 并行子代理
```
"本轮同时 spawn subagents 并行调查 X、Y、Z。
每个子代理独立输出结果。"
```

---

## [[CLAUDE_md_Best_Practices|CLAUDE.md]] 前置战略意图（七组件）

```markdown
# CLAUDE.md for Opus 4.7
1. 正在构建什么
2. 目标用户
3. 禁区（off-limits）
4. 成功标准
5. 整体策略
6. 关键约束
7. 偏好输出格式
```

每次新会话自动加载，后续每条消息只写 per-task intent。

---

## [[Claude_Code_Commands|Plan Mode]] 首选（不看 diff，先审 plan）

```
先输出完整计划（不超过 15 行），我确认后再生成代码 diff。
```

CLI：`/ultraplan "任务描述"` → 浏览器审核 plan → 确认后执行

---

## 关联实体

- [[CLAUDE_md_Best_Practices|CLAUDE.md Best Practices]] — 战略意图前置的规则文件
- [[Agent_Harness_Engineering]] — effort 参数与 Harness 厚度的关系
- [[Claude_Code_Commands]] — Plan Mode 操作（`Shift+Tab`、`/ultraplan`）
- [[Claude_Code_Subagents]] — 4.7 默认子代理变少，必须主动要求 fan out
- [[Claude_Code_Hacks]] — Haiku/Sonnet/Ultrathink 廉价模型路由决策树（Hack #13, #29）

*[Source: raw/Claude Opus 4.7.md]*
