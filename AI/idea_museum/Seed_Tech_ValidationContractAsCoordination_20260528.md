---
name: Validation Contract 是多 Agent 协调协议
description: 在规划阶段写数百条验收断言看似过度工程，但它是多 Agent 并行执行时唯一可扩展的协调协议
type: seed
concept: Validation Contract as Coordination Protocol（验收契约即协调协议）
hook_insight: 你以为写 200 条测试用例是在为"验证"做准备——Factory Missions 的反直觉结论：那 200 条断言不是测试，它们是让 20 个并行 Agent 彼此不冲突的唯一协调语言；规格说明书不是文档，它是分布式锁
wiki_link: "[[Multi_Agent_Architecture]]"
---

# Validation Contract 是多 Agent 协调协议

## 技术核心逻辑

Factory Missions（[[Multi_Agent_Missions_System]]）的最大创新：
**Validation Contract 在规划阶段提前写好，可能包含数百条独立断言。**

传统流程：写代码 → 补测试 → 验收
Factory 流程：定义 Validation Contract → Workers 实现 → Validators 对照契约验收

关键洞察：当 20 个并行 Agent 并发修改同一代码库时，没有任何互斥锁能解决冲突——唯一可扩展的协调机制是：每个 Agent 都在共同对准同一份契约，而非对准彼此。

与 [[Multi_Agent_Architecture]] 中 Immutable State Snapshots 互补：
- 状态不可变解决写冲突
- Validation Contract 解决"什么算完成"的定义冲突

## 优缺点对比

**优势**
- 规划成本一次性支付，执行期间零协调开销
- 契约本身可版本化、可 CI 检查（git diff 看到契约变更立即触发 review）
- Validator Agent 可独立于 Worker Agent 运行，无耦合

**劣势 / 陷阱**
- 前期需要高质量 Orchestrator（慢思考强模型）设计契约——设计成本高
- 契约定义错误的代价被放大：20 个 Agent 都在对准一个错误的目标
- 对需求频繁变化的项目，契约维护成本可能超过其带来的协调收益

[Source: wiki/Multi_Agent_Architecture]
