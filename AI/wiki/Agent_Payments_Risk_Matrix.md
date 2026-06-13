---
title: Agent Payments Risk Matrix
aliases: ["Agentic支付风险分类矩阵", "三层支付风险架构", "Agent_Payments_Risk_Matrix"]
tags: [payments, risk-management, hitl, agentic, decision-framework]
category: agent-engineering
parent: "[[AI_Agent_Payments]]"
created: 2026-05-27
date: "2026-05-27"
---

# Agent Payments Risk Matrix（Agentic支付风险分类矩阵）

Parent: AI_Agent_Payments

> 当前知识缺口被明确识别：AI_Agent_Payments 描述了技术可行性（x402/USDC/200ms结算），Human_In_The_Loop 描述了门禁机制，但两者之间缺少统一的 **决策边界矩阵**。本笔记填补这一缺口。

[Source: raw/制度演化飞轮.md]

---

## 三层风险架构

| 层级 | 风险维度 | 可逆性 | 金额量级 | 对手方可信度 | 决策 | 审计要求 |
|------|----------|--------|----------|--------------|------|----------|
| **只读发现层** | 低 | 高 | 任意 | 任意 | 完全自主 | 无 |
| **小额微支付层** | 中 | 高 | ≤ $100 | 高/中 | 自动 + 事后审计 | 每日汇总 |
| **高风险不可逆层** | 高 | 低 | > $500 或关键资产 | 低/未知 | 强制 HITL | 实时人工审批 |

**设计原则**：参考 [[SAP_Agent_Resilience]] 的"写操作永不静默失败"（NEVER fallback for writes）和 [[Multi_Agent_Architecture]] 的 Reader/Orchestrator/Resolver 分层思路。

---

## 决策逻辑

```
触发支付操作
    ↓
是否为只读查询？→ YES → 完全自主
    ↓ NO
金额 ≤ $100 且对手方可信度 HIGH？→ YES → 自动执行 + 记录日志
    ↓ NO
任一条件：金额 > $500 / 不可逆 / 对手方未知？→ YES → 强制 HITL
```

**核心原则**：
- 可逆性优先于金额阈值（$50不可逆操作 > $500可撤销操作风险）
- 对手方可信度：白名单域名 > 首次交互 > 匿名地址
- 审计日志必须实时写入，支付操作不允许事后补录

---

## 与现有系统的集成点

**技术支付层**（AI_Agent_Payments）：
- x402协议/USDC/M2M支付的实际执行在第2-3层使用
- Bedrock AgentCore Payments 作为第2层的托管方案
- Uniswap AI Suite 属于第2/3层边界案例

**门禁执行层**（[[Human_In_The_Loop]]）：
- 第3层必须通过 HITL 工具调用拦截钩子实现
- 可使用 [[Claude_Code_Hooks]] PreToolUse 物理拦截

**SAP企业层**：
- SAP写操作安全矩阵（SAP_Agent_Resilience）的思路延伸到支付场景
- 高风险层须通过 [[SAP_Agent_Error_Handling]] 的 DeadLetterQueue 记录

---

## 与制度演化飞轮的关系

此矩阵应作为 [[Institutional_Evolution_Flywheel]] 的持久化规则写入 CLAUDE.md：

```markdown
# Payments Orchestration Rules
- 只读查询：完全自主
- 金额 ≤ $100 + 白名单对手方：自动执行
- 金额 > $500 或不可逆：强制 HITL
- 每次支付操作写入审计日志
```

飞轮机制：每次支付异常 → 记录到规则库 → 更新矩阵阈值 → 下次运行约束增强。

---

## 相关笔记

- AI_Agent_Payments — x402协议/USDC技术详情
- Human_In_The_Loop — HITL拦截钩子实现
- SAP_Agent_Resilience — 写操作安全矩阵
- Multi_Agent_Architecture — Reader/Orchestrator/Resolver分层
- Institutional_Evolution_Flywheel — 飞轮规则持久化
- Claude_Code_Hooks — PreToolUse物理拦截
