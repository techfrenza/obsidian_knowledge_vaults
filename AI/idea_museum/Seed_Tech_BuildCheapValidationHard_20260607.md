---
name: Build 越便宜，Validation 越难
description: AI 将 MVP 构建成本压缩至近乎零，但这消除了工程成本作为"验证门槛"的天然过滤器，反而提高了构建错误产品的概率
type: seed
concept: AI 加速创业的验证倒置悖论（Validation Cost Paradox in AI-Native Startups）
hook_insight: AI 把"造"的成本降到接近零——但它同时消灭了逼你想清楚的那道门槛
wiki_link: "[[AI_Native_Startup_Playbook]]"
---

## 技术核心逻辑

传统创业的隐性护栏：高工程成本（2个月工期）迫使创始人在动手前深度验证假设。

AI 时代的变化：
| 过去 | 2026 |
|------|------|
| MVP 成本 = 2-3 个月工程量 | MVP 成本 = 半天 Claude Code |
| 工程成本是天然过滤器 | 过滤器消失 |
| 先验证，再构建（成本逼迫） | 先构建，再验证（惯性驱动） |
| 42% 失败因"无人要" | 比例可能更高（门槛更低，进入更多人）|

核心矛盾：**Prototype 的时间成本 → 0，但用户研究的时间成本不变**

Anthropic 官方数据：42% 创业失败原因是"built something nobody wanted"——这是在 AI 工具之前的比例，AI 时代这个陷阱只会更深。

## 优缺点对比

**AI-Native 创业优势**：
- 极低实验成本，可以快速并行测试多个假设
- 技术门槛降低，领域专家可直接成为创始人

**被忽视的代价**：
- 确认偏误被 AI 放大——Claude 会顺着你的方向找支持证据
- Prototype 太像真产品，用户测试时无法区分"演示惊喜"和"真实需求"
- Agentic 技术债会复利积累，MVP 阶段跳过架构文档的代价在 Launch 阶段统一结算

## 关联

- [[AI_Native_Startup_Playbook]] — 三大反直觉洞察来源
- [[CLAUDE_md_Best_Practices]] — CLAUDE.md 是防止技术债复利的核心机制
- [[Human_In_The_Loop]] — Validation 的本质是不能被 AI 替代的人工判断环节
