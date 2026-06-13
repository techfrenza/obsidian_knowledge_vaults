---
name: GBrain Fat Skill 冲突裁决真空
description: Thin Harness 架构的核心优势（简洁路由）同时是它最大的盲点：Fat Skills 之间的语义冲突没有仲裁层，冲突被静默解决或随机选择
type: seed
concept: Fat Skills + Thin Harness 的裁决真空（Skill Conflict Arbitration Gap）
hook_insight: Harness 越薄越优雅——但当两个 Fat Skill 抢同一个任务时，没有人当裁判
wiki_link: "[[GBrain_Fat_Thin_Architecture]]"
---

## 技术核心逻辑

GBrain 架构核心原则：Thin Harness（只负责路由） + Fat Skills（每个只做一件事）。

| 架构层 | 职责 | 盲点 |
|--------|------|------|
| Thin Harness | 消息 → 匹配 Skill → 执行 | 不处理"两个 Skill 都匹配"的情况 |
| Fat Skills | 独立执行复杂逻辑 | 相互不可见，无法自我感知冲突 |
| resolver.md | 路由表 | 静态规则，无动态优先级仲裁 |

**真实冲突场景**：
- "帮我回顾上周和 John 的会议，并更新他的 CRM 状态" 
- 触发：Meeting-Prep Skill（读 brain page）AND Meeting-Ingestion Skill（写 entity）
- Thin Harness 的答案：取先匹配的，或者都触发（写冲突）

对比 [[Skill_Engineering_10_Rules]] 规则 8（DRY 审核）：
> 引入新技能前，交叉比对所有 `writes_to` 权限路径，避免多 Agent 并发写入脏数据

这条规则存在的原因，恰好证明 Thin Harness 本身无法阻止冲突。

## 优缺点对比

**Fat Thin 架构优势**：
- Harness 几乎零维护成本
- Skill 可独立迭代、Git PR、team review
- 模型无关（Skill 内部自定义模型）

**冲突裁决代价**：
- 冲突预防外包给"人工 DRY 审核"——这是软性约束，不是物理约束
- 随着 Skill 数量增长（100+），冲突组合数呈二次方增长
- [[Harness_Engineering_Advanced]] 的 Throttling Gate 和 One-Shot 原则是补丁，不是架构解法

## 关联

- [[GBrain_Fat_Thin_Architecture]] — 架构核心
- [[GBrain_Architecture]] — GBrain 总体设计
- [[Skill_Engineering_10_Rules]] — 规则 8 是对此问题的软性补丁
- [[MultiAgent_Concurrent_Write_Research]] — 并发写入竞态是同一类问题的数据层版本
