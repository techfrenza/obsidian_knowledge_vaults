---
title: Skill Design Patterns
aliases: ["Agent Skill设计模式", "SKILL.md Patterns", "AI Agent Tips", "Tool Wrapper模式", "Generator模式"]
tags: [skills, skill-patterns, agent-design, workflow, claude-code, prompt-engineering]
category: claude-tooling
parent: "[[index]]"
created: 2026-05-02
date: "2026-05-02"
---

# Skill Design Patterns

Parent: [[index]]

> 核心论点：SKILL.md 质量决定 Agent 可靠性。五种模式覆盖 95% 场景，关键在**分离"检查项"与"检查方式"**，并用 diamond gate 阻止 Agent 跳步。

---

## 五大设计模式

### Pattern 1: Tool Wrapper（让 Agent 瞬间成为特定库专家）

**适用场景**：需要 Agent 遵守特定库/框架的内部规范。

```yaml
Trigger: contains "FastAPI" or "write API" or "review FastAPI"
Instructions:
- ONLY when triggered, load references/conventions.md
- Treat loaded content as absolute truth
- Apply rules to any code write/review task
- NEVER invent conventions
```

`references/conventions.md` 存放团队最佳实践，动态注入上下文。

---

### Pattern 2: Generator（强制输出结构一致）

**适用场景**：API 文档、commit message、项目脚手架等需要格式统一的输出。

```yaml
Trigger: "generate report" or "create doc" or "scaffold"
Instructions:
- Load assets/template.md and references/style-guide.md
- Ask user ONLY for missing variables one by one
- Fill template strictly, no extra text
- Output final document only after all variables collected
```

`assets/` 放可复用模板，`references/` 放风格规则。

---

### Pattern 3: Reviewer（分离检查项与检查方式）

**适用场景**：代码审查、PR audit、安全检查。

```yaml
Trigger: "review code" or "audit" or "check PR"
Instructions:
- Load references/review-checklist.md
- Score each item by severity (Critical/High/Medium/Low)
- Group findings, never skip checklist
- Output structured report only
```

替换 `checklist.md` 即可切换 Python 风格检查或 OWASP 安全检查，无需修改 Skill 主体。

---

### Pattern 4: Inversion（Agent 先面试你再行动）

**适用场景**：项目规划、需求收集，防止 Agent 猜答案。

```yaml
Trigger: "plan project" or "setup" or "new task"
Instructions:
- DO NOT synthesize final output until ALL phases complete
- Phase 1: Ask structured questions one by one
- Wait for user answer before next phase
- Only after full context, output plan
```

强制 gating：`DO NOT start building until all phases complete`。

---

### Pattern 5: Pipeline（强制多步工作流 + 检查点）

**适用场景**：文档流水线、代码生成 + review 等复杂多步任务。

```yaml
Trigger: "run documentation pipeline" or "full workflow"
Instructions:
- Step 1: Generate docstrings → require user confirm
- Step 2: ONLY after confirmation, proceed to assembly
- Load specific references/ at each step only
- Never skip checkpoint or bypass user approval
```

Diamond gate 条件：每步需用户确认才继续，context 保持干净。

---

## 模式选择决策树

```
If need consistent output        → Generator
If need code/library expertise   → Tool Wrapper
If need audit/check              → Reviewer
If need full requirements first  → Inversion
If complex multi-step            → Pipeline
```

直接复制到 `CLAUDE.md` 作为 Skill 选型规则。

---

## 模式组合策略

| 组合 | 用途 |
|------|------|
| Pipeline 内嵌 Reviewer | 生成后自动自我检查 |
| Generator 开头加 Inversion | 先收集变量再填充模板 |
| Tool Wrapper + Reviewer | 遵守规范 + 验证结果 |

---

## SKILL.md 维护规则

- 总长度 **<150 行**（参照 [[Claude_Code_Skills]] 的 <500 行约束，Skill 越短触发越稳定）
- 新 Skill 建好后立即测试：5 种触发词 + 2 种不触发场景
- 每周 review 一次 SKILL.md，删除冗余指令

---

## 关联实体

- [[Claude_Code_Skills]] — 六大 SKILL.md 必要模式（description 写法、负触发优先等底层规则）
- [[Claude_Code_Hooks]] — Hooks 是事件驱动的确定性层；Pipeline 模式的 checkpoint 可通过 Hooks 强制执行
- [[CLAUDE_md_Best_Practices]] — 模式选择决策树建议写入 CLAUDE.md，作为持久规则
- [[Agent_Harness_Engineering]] — Skill 在 Harness 六层架构中的位置（Layer 3），Pipeline 模式对应 Layer 4-5
- [[Prompt_Engineering_Library]] — Generator/Reviewer 模式的模板内容来源（Expert Prompts 直接嵌入 assets/）

*[Source: raw/AI Agent Tips.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图