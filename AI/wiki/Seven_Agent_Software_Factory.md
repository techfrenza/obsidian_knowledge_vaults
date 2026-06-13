---
title: Seven-Agent Software Factory
aliases: ["7-Agent Software Factory", "七Agent软件工厂"]
tags: [multi-agent, software-factory, orchestration, human-in-the-loop, production]
category: agent-engineering
parent: "[[Multi_Agent_Missions_System]]"
created: 2026-05-27
date: "2026-05-27"
---

# Seven-Agent Software Factory（7-Agent软件工厂）

Parent: [[Multi_Agent_Missions_System]]

> 核心理念：将大任务拆解为7个专注型Agent，每个Agent只做一件事，上下文干净，权限受限，通过Human Checkpoints控制质量门。

[Source: raw/7-Agent Software Factory.md]

---

## 7个角色定义

| # | Agent角色 | 权限 | 输出 |
|---|-----------|------|------|
| 1 | **Codebase Researcher** | 只读 | 代码/文档/wiki调研报告 |
| 2 | **Story Writer** | 只读 | 用户故事 + Acceptance Criteria |
| 3 | **Spec Writer** | 只读 | 技术规格书（Blueprint） |
| 4 | **Backend Builder** | 写（后端） | 后端代码实现 |
| 5 | **Frontend Builder** | 写（前端） | 前端代码实现 |
| 6 | **Test Verifier** | 读/执行 | 测试用例 + 执行报告 |
| 7 | **Implementation Validator** | 只读 | Gap分析报告（不做修改，只报告） |

**关键设计原则**：Validator不做修改，只报告差距。这是防止最后步骤引入错误的重要约束。

---

## 目录结构

```bash
.agent/
├── AGENTS.md          # 全局持久化指令（参见下方模板；类比 [[AI_Team_Coding_Practice]] 中的AGENTS.md上下文资产）
├── skills/            # 可复用Skill库
├── workspace/         # 实际工作目录
├── tasks/             # TASK-xxx.md 输入
├── plans/             # PLAN-xxx.md（需人工审批）
├── artifacts/         # 输出物（报告/代码/QA）
├── templates/
│   └── QA_Report.md   # QA模板
└── logs/              # 执行日志
```

---

## 核心工作循环

```
创建 TASK.md
    ↓
Orchestrator 生成 PLAN.md
    ↓ ← 人工 /approve 审批
按顺序激活7个Agent
    ↓
Test Verifier + Implementation Validator
    ↓
生成 QA Report + Release Notes
    ↓
归档到 artifacts/ + 更新 _history/runs.md
```

**Human Checkpoints**：
- PLAN.md 生成后 → 等待 `/approve [TASK-ID]` 或 `/reject [TASK-ID]`
- QA Report 生成后 → 等待人工确认再发布

---

## AGENTS.md 模板片段

```markdown
# 7-Agent Software Factory 全局规则
可用Agent：Researcher / Story Writer / Spec Writer / Backend Builder / Frontend Builder / Test Verifier / Validator
必须遵守：
- 每个关键步骤生成PLAN.md等待人工/approve
- Agent_Payments_Risk_Matrix 三层风险规则（涉及支付时）
- 输出Objective and Fact-driven
- 高风险操作强制HITL
```

---

## 与现有架构的对比

| 维度 | 7-Agent Factory | [[Multi_Agent_Missions_System]] Factory Missions |
|------|-----------------|------------------------------------------------|
| 角色粒度 | 7个固定角色 | 动态Orchestrator/Workers/Validators |
| HITL频率 | 每个关键步骤 | Validation Contract First |
| 适用场景 | 完整功能开发 | 通用多Agent任务编排 |
| 权限分离 | 严格（Validator只读） | 较灵活 |

---

## 相关笔记

- [[Multi_Agent_Missions_System]] — Factory Missions通用架构
- [[Multi_Agent_Architecture]] — 三层分离模式
- [[Human_In_The_Loop]] — HITL实现机制
- [[Skill_Engineering_10_Rules]] — 配合使用的Skill工程规则
- [[Harness_Engineering_Advanced]] — repo级别Harness实现
