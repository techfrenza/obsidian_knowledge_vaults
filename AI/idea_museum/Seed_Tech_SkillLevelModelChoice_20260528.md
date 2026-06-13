---
name: Seed_Tech_SkillLevelModelChoice_20260528
description: GBrain架构中模型选择权被下沉到Skill层而非Harness层，实现真正的模型无关性——同一Harness下不同Skill可调用不同Provider/模型族
type: seed
concept: Skill级模型选择权（Skill-Level Model Sovereignty）
hook_insight: "你把模型选择写在 CLAUDE.md 里——这意味着整个 Harness 只能用一个模型。GBrain 的反设计是把模型选择下沉到每个 Skill：precision 任务用 Opus，recall 任务用 GPT，creative 任务用 DeepSeek，全部在同一个 Harness 下。Harness 应该对模型完全无知，就像 OS 对 CPU 无知一样"
wiki_link: "[[GBrain_Fat_Thin_Architecture]]"
---

# Skill级模型主权：真正的模型无关Harness设计

## 技术核心逻辑

大多数 Harness 设计：**模型选择在 Harness 层**（CLAUDE.md 或配置文件指定单一模型）
- 等效于：所有 Skill 共享同一个"智能引擎"
- 问题：一旦换模型，所有 Skill 行为都可能漂移

GBrain 架构反设计：**模型选择在 Skill 层**
```
Thin Harness（路由，完全模型无关）
  ↓ 触发
Fat Skill（内部决定调用哪个模型）
  - precision 任务 → Opus 4.7
  - recall 任务   → GPT-5.5
  - creative 任务 → DeepSeek R2
  - 互评验证       → Opus + GPT + DeepSeek 同时调用
```

## 反直觉权衡

| 架构 | 模型选择位置 | 优势 | 劣势 |
|------|------------|------|------|
| 常规 | Harness 层 | 管理简单，单一维护点 | 模型升级影响全局，无法针对任务优化 |
| GBrain | Skill 层 | 每 Skill 优化模型选择，Provider 冗余 | 需要每个 Skill 作者有模型知识 |

## 深度洞察

书 Book-Mirror Skill 示例：Opus 4.7 做精度（提取章节核心论点）+ GPT-5.5 做召回（发现跨章节联系）+ DeepSeek 做创意（映射到个人脑库）——三者并行，结果互相评估。单一模型无法同时优化精度、广度和创意；Skill 层模型选择让"多模型互评"成为可能。

这等效于：每个 Skill 都是一个独立的小型 AI 系统，Harness 只是路由器。真正的模型无关性不是"随时能换模型"，而是"不同任务用最适合的模型"。

[Source: wiki/GBrain_Fat_Thin_Architecture]
