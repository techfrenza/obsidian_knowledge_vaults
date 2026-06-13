---
title: SAP Agent Skills
aliases: ["SAP skills", "SkillLifecycleManager"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, skills, SKILL-md, skill-loader, enterprise-AI]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Skills

SKILL.md is the agentskills.io open standard for reusable agent skills. Adopted by Claude Code, Cursor, Gemini CLI, GitHub Copilot, VS Code, and 30+ tools. SAP agents use it for progressive skill disclosure.

## SKILL.md Frontmatter

```yaml
name: data-validator          # 1-64 chars, lowercase+hyphens, MUST match directory name
description: |                 # 1-1024 chars — says WHAT and WHEN to use
  Validates SAP journal entry data before submission...
license: SAP-internal
compatibility:
  claude-code: ">=1.0"
metadata:                      # Enterprise custom keys
  version: "1.2.0"
  domain: finance
  author: team-fin-agents
  dependencies: [gl-account-validator]
allowed-tools: [Read, Bash]   # experimental
```

## Enterprise Body Sections

```markdown
## When to Use
## Input Requirements
## Step-by-Step Process
## Output Format
## Edge Cases
## Examples
```

Keep SKILL.md under 500 lines (~5000 tokens). Move details to `references/`, `scripts/`, `assets/`.

## SkillLifecycleManager (`core/skill_loader.py`)

Progressive disclosure — load metadata only at startup; full content only when activated:

```python
manager.discover()                                   # startup: ~50 tokens metadata
await manager.activate("data-validator", ctx_id)    # on-demand: full SKILL.md ~2000 tokens
await manager.execute("data-validator", "validate", ctx_id, data=payload)
manager.deactivate("data-validator", ctx_id)        # explicit deactivation
manager.deactivate_all(ctx_id)                      # end of conversation cleanup
```

**Context protection**: `compact_context()` exempts `<active_skill>` messages from compaction — preserves active skill instructions even when conversation history is compressed.

## Three Activation Models

### 1. Model-Driven
LLM reads `<available_skills>` catalog → autonomously calls file-read tool to load `SKILL.md`. Flexible but unpredictable — not recommended for enterprise write operations.

### 2. Harness-Driven (Enterprise)
Rule engine pre-selects mandatory skills, injects directly into context. **Deterministic, testable, auditable** — preferred for compliance-critical workflows.

### 3. Combined Pattern (Recommended)
Mandatory skills: harness-driven. Optional/situational skills: catalog available for LLM activation. Best of both worlds.

## Skill Trust Levels

| Level | Auto-load | Logging |
|---|---|---|
| Bundled | ✅ Auto-load | Standard |
| Internal shared | ✅ Auto-load | Log provenance |
| External | ❌ Require explicit approval | Full audit |

## Finance Domain Skills

Built-in skills for finance agents:
- `data-splitter` — splits upload CSV into journal entry batches
- `data-validator` — validates line items before submission
- `clearing-matcher` — finds AP clearing proposals
- `amount-calculator` — handles multi-currency calculations
- `document-creator` — structures OData payload
- `fiscal-period-checker` — validates period is open
- `exchange-rate-converter` — fetches and applies exchange rates

## ConditionalSkillSelector

Rule-based mandatory skill selection overlaying model-driven activation. Maps intent → required skills:
```python
selector.for_intent("CREATE_JOURNAL_ENTRY") 
# → mandatory: [data-validator, fiscal-period-checker, document-creator]
```

## Subagent Delegation

Complex skills run in isolated subagent session with only skill content + query — protects main context from skill bloat.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Prompt_Engineering]] — skill injection into Layer 4 of system prompt
- [[SAP_Agent_Testing]] — testing skill behavior with TestModel
- [[Claude_Code_Skills]] — Claude Code skill system (same SKILL.md standard)
- [[Skill_Design_Patterns]] — general skill design
- [[Skill_Ecosystem]] — skill ecosystem and sharing

[Source: raw/SAP/skills-deep-dive.md]
