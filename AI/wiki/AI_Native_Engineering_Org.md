---
title: "AI-Native Engineering Org"
parent: "[[Enterprise_Agent_Playbook]]"
tags: [engineering-org, ai-native, process-transformation, bottleneck-shift]
category: ai-native-org
date: "2026-06-07"
source: "raw/Running an AI-native engineering org.md"
---

# AI-Native Engineering Org

AI-native 工程组织的核心命题：**编写代码不再是瓶颈，验证、审查和决定"构建什么"才是。** 当 agentic coding 成为默认工作方式，所有围绕"代码编写成本高昂"而设计的流程都需要重写。

[Source: raw/Running an AI-native engineering org.md]

---

## 流程重写：四大变革

| 旧范式 | 新范式 | 原因 |
|--------|--------|------|
| 六个月路线图（因编码成本高而重度预规划） | JIT（Just-In-Time）规划：原型 → 内部用户 → 反馈迭代 | 速度变了，规划粒度必须变 |
| "找写代码的人问" | 先问 Claude，再判断是否可自动化 | AI 辅助 PR 成为默认，作者身份不再重要 |
| 人类审查所有代码 | Claude 处理风格/lint/bug/测试；人类审查高价值节点（法律/安全/产品判断力） | 人的稀缺资源集中于无法被 AI 替代的判断 |
| 角色固定（工程师写代码/PM 规划/设计师设计） | 角色模糊：PM 写代码，工程师做内容和设计 | AI 降低了跨领域门槛 |

---

## 三大核心原则（Anthropic 内部不可商议规则）

1. **无情地将产品 dogfood**：每位团队成员（含跨职能伙伴）都使用 Claude Code + Claude Cowork，不断寻找让 AI 加速自己工作的方式。
2. **保持团队扁平**：管理者先做 IC，学会有效工程，再管理；维护统一团队使命，让人员跟随工作流动。
3. **明确授权杀死无效流程**：当某个流程不再有意义，团队成员有权质疑并废弃它。

---

## 三个关键追踪指标

| 指标 | 含义 |
|------|------|
| 新员工上手时间缩短 | 工程师第一周内即可 ship 真实代码 |
| PR 周期时间缩短 | 排查瓶颈（CI 系统可能成为新瓶颈） |
| Claude 辅助提交比例上升 | 默认每个 commit 都是 Claude 辅助的 |

> 警告：吞吐量不等于成功。真正的指标是你试图解决的问题。

---

## JIT 规划（Just-In-Time Planning）

类比 JIT 编译：**在恰当的时机做恰当程度的规划**。

- 从设计文档转向 PR 内讨论或原型
- 不做大量前期产品评审，直接原型化 + 内部用户验证 + 快速迭代
- "在技术争论中，代码赢"：当队员对设计方向有争议时，最快的解法是让 Claude 分别原型两种方案，审查实际产出物

---

## 团队角色转型信号

- **PM 大量写代码**：AI 降低了工程门槛，非传统 coder 获得工程能力
- **工程师承接内容/设计**：传统上属于非技术侧的工作
- **招聘偏好转向**：重产品感知的创意型建造者 + 深度系统专家；减少对"原始吞吐量"的偏重（模型负责这部分）

---

## 瓶颈转移：从写代码到验证

> "The bottleneck moved from coding to everything around coding." — Fiona Fung（Anthropic Claude Code 工程团队）

新瓶颈三类：
1. **验证容量**：代码生成速度 300%+，单个文档工程师成为瓶颈案例（Noah Zweben 数据：3个月内周 PR 从 500 → 1150）
2. **代码审查**：人类如何跟上 AI 生成速度？答案：关注领域专业知识而非覆盖率
3. **决定构建什么**：瓶颈从外部（技术/资金/人手）整体迁移到内部（判断力/自律/诚实）

---

## 异步 Agent 协作模式

- **隔夜运行 Agents**（Daisy Hollman，Anthropic MTS）：6 个月前听起来激进，现在已可行，支持 Opus 4.7 自动模式运行数小时
- **双 Agent 代码审查**：CodeRabbit 处理风格/惯例，Claude Code 处理代码链和下游影响，两者"辩论"PR，人类审查最终输出
- **Robobun 案例（Jarred Sumner）**：Robobun 对 Bun 代码库的 commit 数量已超过创始人本人

---

## 与其他概念的关系

- [[Enterprise_Agent_Playbook]] — 企业 AI 转型策略层（本页是工程执行层）
- [[Seven_Agent_Software_Factory]] — 7 Agent 工厂的角色分工
- [[Context_Engineering]] — JIT 规划依赖上下文工程最优化
- [[Human_In_The_Loop]] — 新流程中人类审查的明确位置
- [[Claude_Code_Routines]] — 隔夜自动化 + 定时任务实现
- [[Production_Agent_Engineering]] — 生产 Agent 四大原语
- [[Harness_Engineering_Deep_Dive]] — Agent Harness 完整工程基础设施（异步 Agent 依赖的基础层）
- [[Claude_Code_Advanced_Features]] — 大型代码库工作流（Anthropic 内部 7 步循环）
- [[Agent_Engineer_MOC]] — AI-Native 工程师学习路径
