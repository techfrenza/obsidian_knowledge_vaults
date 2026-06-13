---
name: Drift Linter — CI as Agent Quality Gate
description: Skill 漂移检测作为 CI 门禁：Agent 行为一致性与代码一致性在同一流水线中强制执行
type: seed
---

# Drift Linter — CI as Agent Quality Gate

[Concept] **代码漂移检测 = Agent 行为漂移检测**

传统 CI：代码质量（lint / type check / test）
Drift Linter 扩展：Skill 文件同步一致性 → **构建失败如有任何 Agent Skill 副本与主库分叉**

[Trade-off]

| 方式 | 优 | 劣 |
|------|----|----|
| 无漂移检测 | 灵活，各 Agent 可自定义 Skill | Skill 版本碎片化，相同任务产出不一致 |
| Drift Linter in CI | Skill 单源权威，Agent 行为可预测 | Skill 更新需走 PR 流程，速度稍慢 |

关键洞见：**你测代码质量，但你没有测 Agent 指令质量**。Skill 漂移是系统性幻觉的根源之一——不同副本的 Agent 在"按规行事"，但规不一样。

[Wiki Link] [[Multi_Agent_Architecture]] · [[Claude_Code_Skills]] · [[Skill_Design_Patterns]]
