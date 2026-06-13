---
title: "Loop Engineering"
parent: "[[Agentic_Loop]]"
tags: [loop-engineering, feedback-loop, closed-loop, open-loop, agent-reliability]
source: "raw/Loop Engineering - Agent时代最被低估的核心能力 —— 从 Prompt 到可靠闭环系统的实战指南.md"
---

# Loop Engineering

**Loop Engineering 是 Prompt Engineering 的上位替代**：Prompt 决定起点，Loop 决定上限。顶级 AI 工程师不是最会写 Prompt 的人，而是最会设计反馈循环的人。

[Source: raw/Loop Engineering - Agent时代最被低估的核心能力 —— 从 Prompt 到可靠闭环系统的实战指南.md]

> Boris Cherny（Claude Code 负责人）："我不再 Prompt Claude，我让 Loops 去 Prompt Claude。我的工作是写 Loops。"

---

## 为什么 Loop 优于 Prompt

| 维度 | 传统 Prompt | Loop Engineering |
|------|------------|----------------|
| 模式 | 一次性函数：输入→输出→结束 | 反馈循环：行动→观察→修正→重复 |
| 容错 | 无验证，容易 hallucination | 自我修正至客观标准通过 |
| 分数 | 聊天框内 60 分 | 同一模型放入好 Loop 后达 90 分 |
| 成本 | 低（单次调用） | 高（单次 50k-200k+ token），需低价大上下文模型 |

**同一个模型，聊天框里 60 分，放进好 Loop 后可达 90 分。**

---

## 两种规模 × 两种类型

**规模**：

| 类型 | 适用场景 |
|------|---------|
| Single-Agent Loop | 一个 Agent 自我迭代，适合简单、专注任务 |
| Fleet Loop | Orchestrator + Specialists + Subagents，适合复杂项目（如完整 App） |

**类型（最重要区分）**：

| 类型 | 特征 | 适用 |
|------|------|------|
| **Open Loop** | 探索性，自由试错，强大但极耗 Token、易产 Slop | 预算无限时 |
| **Closed Loop** | 边界清晰、有明确步骤+质量门（Gate），可靠、可控、预算友好 | **大多数场景首选** |

推荐路径：先建可靠 Closed Loop，再逐步开放。

---

## 五阶段执行结构

```
DISCOVER → PLAN → EXECUTE → VERIFY → ITERATE（直到通过 Gate）
```

每个循环都经历完整五阶段，Gate 未通过则重新进入 DISCOVER/PLAN。

---

## 六个构建块

| 块 | 作用 | 工具 |
|----|------|------|
| **Automations（心跳）** | 触发发现，设定 cadence 和停止条件 | `/loop`、`/goal`、Scheduled Routines、Cloud Routine |
| **Worktrees（并行隔离）** | 避免多 Agent 文件冲突 | `--worktree` 或 `isolation: worktree` |
| **Skills（项目知识）** | 让 Agent 每次都"记住"规范 | SKILL.md + VISION.md + ARCHITECTURE.md + RULES.md |
| **Plugins & Connectors（MCP）** | 连接真实工具 | GitHub、Linear、Slack、Database |
| **Subagents（职责分离）** | Maker（写）vs Checker/Verifier（审），避免自我偏好 | [[Claude_Code_Subagents]] |
| **Memory/State（持久化）** | 记录历史、教训、进度（Agent 忘，文件不忘） | STATE.md 或外部系统 |

---

## 4 条件测试（是否值得建 Loop）

未通过任一条 → 继续手动 Prompt：

1. 任务重复（至少每周）
2. 有自动化验证（测试、linter、build）
3. Token 预算能承受浪费
4. Agent 有运行/观察环境的工具

---

## 14 步实战路线图

**Tier 1：评估**
1. 理解 Loop 是"替换自己作为 Prompter"
2. 跑 4 条件测试
3. 评估经济性（DeepSeek/Kimi 等低价模型跑重 Loop）
4. 30 秒检查清单

**Tier 2：掌握构建块**
5. Automations（/loop + /goal）
6. Worktrees
7. Skills（示例：CI Triage Skill）
8. Connectors（GitHub、Linear 等）
9. Subagents（Evaluator-Optimizer 模式）

**Tier 3：正确构建**
10. 建立 State File
11. 构建 **最小可行 Loop（MVL）**：1 Automation + 1 Skill + 1 State + 1 Gate
12. 避免 Ralph Wiggum Loop（软 Gate、无客观验证）
13. 防范 Comprehension Debt（必须读 Diff、Spot-check Gate）
14. 处理 Security Tax（权限审计、日志清理）

---

## 真实 Loop 示例

**Coding Loop（最典型）**：
```
Read VISION.md → Plan → Edit → Run tests → Fix if fail → Summarize
Stop when: tests + lint pass
```

**Claude Code 实现**：
```bash
/loop 30m /goal "all tests pass and lint clean"
```

配合：`SKILL.md`（background: true + context: fork）+ `/tasks` 监控。

---

## 致命错误清单

避免以下错误（Token 钱坑）：

- 无客观 Gate（"输出看起来不错"不算 Gate）
- Maker = Checker（自我评审等于没评审）
- 无 State File（每轮重新开始，无法累积）
- 无预算上限（Open Loop 无止境消耗）
- 在判断性工作上用 Loop（创意决策不适合 Loop）
- 不读 Loop 输出（Cognitive Surrender）

---

## 维护原则

- 始终读 Diff、Spot-check Gate
- 定期 Audit Skills/权限
- 保持工程师身份：Loop 是杠杆，不是替代思考

**Build the loop. But stay the engineer.**

---

## 与其他概念的关系

- [[Agentic_Loop]] — 技术机制层：Anthropic SDK 中 agentic loop 的四阶段实现
- [[Claude_Code_Routines]] — 云端自动化 Trigger：Loop 的异步执行调度层
- [[Claude_Code_Subagents]] — Maker vs Checker 分离：Loop 中的职责隔离实现
- [[Harness_Engineering_Advanced]] — Harness 作为 Loop 的控制平面：Plan-First 工作流 + 熔断机制
- [[Skill_Engineering_10_Rules]] — Skills 是 Loop 的项目知识层（构建块 3）
- [[Context_Engineering]] — State File 管理：Loop 运行期间上下文治理与 Context Rot 防治
- [[Agentic_Memory_System]] — State File = External Memory 在 Loop 中的持久化实现
- [[CLAUDE_md_Best_Practices]] — VISION.md/RULES.md 文件设计：Loop 中 Skills 的规范来源
- [[Generator_Evaluator_Separation]] — Subagent 职责分离的具体实现：Maker vs Checker 在代码评审场景
- [[Multi_Agent_Architecture]] — Fleet Loop 的编排基础
