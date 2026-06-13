---
title: Claude Code Hooks
aliases: ["Hooks 确定性约束", "postEdit hook", "pre_tool_call"]
tags: [hooks, claude-code, automation, deterministic, security]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Code Hooks

Parent: [[index]]

> 核心论点：Hooks 是 Claude Code 的**确定性执行层**。把 Claude"记不住的事"通过系统层强制执行。适合放 Hooks 的：Edit 后自动 lint/format、保护文件阻断修改、SessionStart 注入动态环境。

---

## Hooks 适用 vs 不适用

| 适合 Hooks | 不适合 Hooks |
|------------|--------------|
| Edit 后自动 lint/format | 复杂语义判断（放 [[Claude_Code_Skills|Skill]]）|
| 保护文件阻断修改 | 长时间流程（放 [[Claude_Code_Subagents|Subagent]]）|
| SessionStart 注入动态环境 | 需要 LLM 推理的决策 |
| 高危命令拦截 | |

---

## 配置位置

```
.claude/hooks/
├── pre_edit_hook.py
├── post_edit_hook.py
└── on_failure.py
```

settings.json 中声明：
```json
{
  "hooks": {
    "postEdit": [
      "prettier --write {file}",
      "eslint --fix {file}"
    ]
  }
}
```

---

## 核心模板

### postEdit 自动格式化
```json
"hooks": {
  "postEdit": ["prettier --write {file}", "eslint --fix {file}"]
}
```

### pre_edit 保护文件
```python
# pre_edit_hook.py
if file in protected_paths:
    raise BlockEdit("受保护文件，需人工确认")
run_lint_and_format()
```

### 高危命令拦截
```json
{"high_stakes": ["git push --force", "deploy"], "requires_approval": true}
```

### on_failure 失败转学习
```python
def on_failure(context, error):
    skill = context.get("skill", "unknown")
    failure_count = context.get("failure_count", 0)
    if failure_count >= 3:
        with open(".agent/memory/failing_skills.md", "a") as f:
            f.write(f"- {skill}: failing ({error})\n")
```

---

## Tool Output 噪声过滤（RTK 模式）

```bash
# Hook 自动截断高输出命令
cargo test --quiet | head -30
```

效果：避免 `git log`、`cargo test` 等几千行输出污染上下文。

---

## Hooks + Skills + CLAUDE.md 三层叠加

```
CLAUDE.md        → 持久规则（What to do）
Skills (.md)     → 工作流程（How to do）
Hooks (scripts)  → 确定性执行（Force done）
```

> 三层说明：[[CLAUDE_md_Best_Practices|CLAUDE.md]] 定义规则、[[Claude_Code_Skills|Skills]] 封装流程、[[Claude_Code_Hooks|Hooks]] 强制执行——叠加后漏洞最少。

三层同时生效，漏洞最少。

---

## 关联实体

- [[Claude_Code_Skills]] — Skill 处理复杂语义逻辑，Hooks 处理确定性执行
- [[Claude_Code_Settings]] — settings.json 中配置 hooks 和 permissions
- [[Agent_Harness_Engineering]] — Hooks 在 Harness 六层架构中的位置（Layer 4）
- [[CLAUDE_md_Best_Practices|CLAUDE.md Best Practices]] — 规则文件，与 Hooks 协同

---

## Hooks vs Skills 核心区分（补充）

| 维度 | Hooks | Skills |
|------|-------|--------|
| 触发方式 | **事件驱动**，自动触发 | **请求驱动**，Agent 主动调用 |
| Agent 感知 | 无需 Agent "思考" | Agent 决策是否需要 |
| 典型场景 | 保存文件、工具调用前后 | 需要特定领域知识 |
| Human-in-loop | 可在高危操作前暂停请求人工确认 | 不涉及 |

> Hooks 在 Agent SDK 中可配置"挂起"循环，在删除/部署等高危操作前强制请求人工许可。

*[Source: raw/Best practice to use Claude code.md, raw/Claude Code 系统治驭工程指南.md, raw/Claude Code Hook.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图
- [[Production_Reliability_MOC]] — 生产可靠性三维度（可见/结构/安全）知识地图