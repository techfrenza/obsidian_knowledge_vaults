---
title: "Generator-Evaluator Separation"
parent: "[[Claude_Code_Self_Evolving]]"
tags: [evaluator, generator, code-review, separation-of-concerns, worktree]
source: "raw/Claude Code 中实现独立 Evaluator 的三种实用方案.md"
---

# Generator-Evaluator Separation

**核心原则**：Generator 与 Evaluator 必须完全隔离。让 Evaluator 看到生成代码时的完整实现上下文，会导致"作者视角偏差"——它会根据意图判断而非实际表现评估。每次从干净的新会话开始，只基于代码实际行为和 Golden Principles 评审。

[Source: raw/Claude Code 中实现独立 Evaluator 的三种实用方案.md]

---

## 三种实现方案

### Option 1 — 独立 CLAUDE.evaluator.md（推荐用于正式评审）

**适用**：需要稳定、可重复使用的评估角色时。

**步骤**：

1. 在仓库根目录创建 `CLAUDE.evaluator.md`：

```markdown
# Evaluator Role

You are an independent code evaluator for this repository.
Your job is to review, critique, and score — NOT to write or fix code.

Rules:
- Never modify files. Read-only mode only.
- For every review, score each Golden Principle (1–5) and explain the gap.
- Surface violations of layered architecture, missing tests, or stale documentation.
- Output a structured report: PASS / WARN / FAIL per dimension.
- Do not defer to the author's intent — evaluate strictly based on what the code actually does.
```

2. 启动独立评估会话：

```bash
claude --system-prompt "$(cat CLAUDE.evaluator.md)"
```

---

### Option 2 — /clear + 内联角色切换（最快速）

**适用**：快速 spot-check、日常迭代后的简单评审。

**步骤**：

1. 当前 Claude Code 会话中输入 `/clear`（重置上下文，清除生成者角色残留）
2. 发送以下消息作为新会话第一条 prompt：

```
You are now acting as an independent Evaluator. Do not write or modify any code.
Your job: Review the last set of changes against the Golden Principles defined in AGENTS.md 
and produce a structured PASS/WARN/FAIL report for each principle.
Score each Golden Principle from 1 to 5 and clearly explain any gaps.
```

`/clear` 确保之前的生成者上下文不污染评估判断。

---

### Option 3 — Worktree + 新会话（最强隔离，推荐用于 PR 评审）

**适用**：需要完全隔离、评估 PR diff 或重要变更时。

**步骤**：

```bash
git worktree add .claude/worktrees/eval HEAD
cd .claude/worktrees/eval
claude  # 新会话中粘贴 Evaluator prompt
```

这是完全独立的只读工作树，Evaluator 几乎不可能意外修改主分支代码。

---

## 方案对比

| 场景 | 推荐方案 | 优点 | 缺点 |
|------|---------|------|------|
| 快速 spot-check（冲刺后检查）| Option 2（/clear + 角色切换）| 最快，无需额外文件 | 隔离性稍弱 |
| 正式 review gate（合并前评审）| Option 1（CLAUDE.evaluator.md）| 可重复使用，启动方便 | 需维护额外文件 |
| 完整隔离 + 评估 PR diff | Option 3（worktree + 新会话）| 最高隔离性，几乎零风险 | 操作稍复杂 |

---

## 隔离性原则

无论使用哪种方案，Evaluator 会话必须保持完全独立：

- 永远不要让 Evaluator 看到生成代码时的完整实现上下文
- 每次从**干净的新会话**开始，基于代码实际表现和 Golden Principles 评判，而非"作者原本的意图"

这是 [[Loop_Engineering]] 中 Maker vs Checker 分离原则在代码评审场景的具体实现：Evaluator-Optimizer 模式要求两个角色使用独立上下文。

---

## 关联实体

- [[Claude_Code_Self_Evolving]] — 自进化循环中 Evaluator 是质量门控的独立角色
- [[Loop_Engineering]] — Maker（Generator）vs Checker（Evaluator）是 Loop 中的职责分离原则
- [[Harness_Engineering_Advanced]] — Harness 中的双 Agent 架构（Writer/Reviewer）
- [[Claude_Code_CLI_Reference]] — `--system-prompt` flag 用于 Option 1 启动方式
- [[Claude_Code_Subagents]] — 上下文隔离机制是 Evaluator 独立性的底层保障
- [[Multi_Agent_Architecture]] — Generator/Evaluator 是 Multi-Agent 质量闭环的基本模式
