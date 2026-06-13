---
type: seed
source: wiki_scan
date: 2026-05-06
---

# Seed: Hooks vs Skills — 层混淆的系统代价

**[Concept]** 把语义判断放进 Hook = 确定性层污染；把格式化放进 Skill = 请求驱动的不可靠性

**核心技术逻辑**

[[Claude_Code_Hooks]] 和 [[Claude_Code_Skills]] 在 Harness 中占不同层，混用会导致可预测性崩溃：

```
正确分层：
  Hooks  → 事件驱动，无需 Agent 思考，100% 确定执行
  Skills → 请求驱动，Agent 决策是否调用，可能跳过

混淆后果：
  语义判断放 Hook → Hook 脚本引入 LLM 调用 → 确定性消失
  格式化放 Skill  → Agent 忘记调用 → 代码风格漂移，无人发现
```

**权衡核心**：
- Hook 的价值在于"Agent 记不住的事通过系统层强制执行"——一旦 Hook 本身需要 LLM 推理，这个保证就消失了
- Skill 的价值在于"封装复杂判断"——如果只是机械格式化，用 Skill 等于给确定性任务加了一个不确定的触发器

**非显而易见点**：很多团队把 `lint` 写进 Skill 而非 Hook，然后在 code review 时发现格式不一致——根因不是规则没写，而是放错了层。

**[Wiki Link]** [[Claude_Code_Hooks]] — Hooks vs Skills 核心区分表  
**[Wiki Link]** [[Skill_Design_Patterns]] — Skill 适用模式决策树
