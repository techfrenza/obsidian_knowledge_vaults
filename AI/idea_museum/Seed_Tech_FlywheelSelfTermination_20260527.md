---
name: Evolution Flywheel Self-Termination
description: 制度演化飞轮通过持续增殖规则来降低错误率，但飞轮本身会耗尽它运行所依赖的资源（CLAUDE.md 注意力容量）
type: seed
concept: 飞轮自我摧毁的内在张力
hook_insight: "制度演化飞轮的设计目标是持续改进——但它通过增殖规则来改进，而规则的增殖最终会使规则本身失效。飞轮转得越好，越接近停止"
wiki_link: "[[Institutional_Evolution_Flywheel]]"
---

## 技术核心逻辑

制度演化飞轮的逻辑链：
```
错误 → 规则更新 → 错误率下降 → 新错误暴露 → 更多规则 → …
```

但存在一个硬上限约束（[[CLAUDE_md_Best_Practices]]）：
- CLAUDE.md < 65 行：高合规率
- CLAUDE.md > 200 行：合规率急剧下降
- CLAUDE.md > 4000 tokens：合规率 ~30%

**内在张力**：飞轮的"输出"（规则文件增长）会侵蚀飞轮"运行所需的基础设施"（CLAUDE.md 注意力容量）。这是一个自我消耗系统，没有剪枝机制就会耗尽自身。

## 两种解读

| 视角 | 含义 |
|------|------|
| 飞轮设计者视角 | 飞轮必须配备"剪枝器"——每新增一条规则，必须有机制删除一条旧规则 |
| 系统论视角 | 飞轮不是单向正反馈，而是带阻尼的振荡系统：增殖→饱和→剪枝→增殖 |

**关键区别**：
- **速度飞轮**（GBrain/Enterprise Playbook）：快速 Skillify，先运转后剪枝
- **质量闸门**（Skill Engineering 10 Rules）：每个 Skill 必须通过 10 步闭环
- 两者都对——飞轮需要速度，但阻止自我摧毁需要闸门

## 权衡

| 快飞轮 | 慢飞轮 |
|--------|--------|
| 规则快速积累，覆盖更多场景 | 每条规则质量更高，持续时间更长 |
| 需要更频繁剪枝（每周） | 剪枝周期可以更长（每月） |
| 适合快速迭代项目 | 适合长期稳定的生产系统 |

*[Source: wiki/Institutional_Evolution_Flywheel.md, wiki/CLAUDE_md_Best_Practices.md]*
