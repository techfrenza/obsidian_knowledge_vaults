---
name: 七 Agent 工厂：Validator 只读不写的反直觉约束
description: 最终验证 Agent 被明确禁止写操作，只能报告差距——这是防止最后一步引入新错误的关键工程约束
type: seed
concept: 只读 Validator 原则（Read-Only Validator Constraint）
hook_insight: 你让最终审查员同时有修复权——这是直觉，但也是生产事故的来源。Seven-Agent Factory 的铁律：Validator 发现问题只能写报告，不能动代码；最后一个有写权限的人，恰好是最容易引入回归 bug 的人
wiki_link: "[[Seven_Agent_Software_Factory]]"
---

# 七 Agent 工厂：Validator 只读不写的反直觉约束

## 技术核心逻辑

[[Seven_Agent_Software_Factory]] 的 Agent 7（Implementation Validator）设计原则：

```
权限: 只读
职责: Gap 分析报告
明确禁止: 不做修改
```

为何重要：
- 软件开发中，越靠近发布的修改越难追踪
- Validator 有修复权时，倾向于"先修再报"，绕过了 Test Verifier（Agent 6）的验证循环
- 将发现与修复分离，迫使 Bug 回流到正确的负责 Agent（Backend Builder 或 Frontend Builder），保持责任归属清晰

与 [[Multi_Agent_Architecture]] 中 Security Tier 设计同构：
- Reader Tier → 只读，防注入
- Resolver Tier → 只写已验证数据
- 没有任何 Tier 同时拥有读不可信源 + 写目标系统的双重权限

## 优缺点对比

**优势**
- 缺陷可追溯：每个 bug 都有明确的责任 Agent
- 防止"最后一分钟热修复"绕过 QA 门控
- Validator 报告可作为 Release Notes 的原始输入，无需额外加工

**劣势 / 陷阱**
- 修复周期变长：发现问题 → 回流 → 重新执行 Agent 4/5/6 → 再验证
- 小型项目中，这种严格分离的协调成本可能超过收益
- 需要 Orchestrator 正确理解 Validator 报告并路由修复任务，增加编排复杂度

[Source: wiki/Seven_Agent_Software_Factory]
