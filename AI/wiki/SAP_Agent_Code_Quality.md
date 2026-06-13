---
title: SAP Agent Code Quality
aliases: ["Vibe Code Reviewer", "SAP code review"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, code-quality, vibe-coding, refactoring, LLM-tools]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Code Quality

Patterns for reviewing and maintaining agent codebases produced via LLM-assisted "vibe coding." Two main tools: Vibe Code Reviewer and God File Decomposer.

## The Problem

Vibe coding produces working agents fast but accumulates predictable technical debt:
- God `agent.py` (500+ lines with mixed responsibilities)
- Hardcoded system prompts, model names, URLs
- Keyword routing instead of intent classification
- No guardrails, no loop termination
- Duplicated skill logic across agent methods
- Bare `except Exception: pass`

## Build → Review → Decompose Cycle

```
Build (fast, with LLM assistant)
  → Review (Vibe Code Reviewer)
    → Decompose (God File Decomposer if god files found)
      → Build more
```

## Vibe Code Reviewer

Turns coding agent into a meticulous auditor. Run it by pasting `prompts/vibe-code-reviewer.md` into system prompt then sending:
```
Review the Python codebase in the app/ directory. Analyze all .py files for code quality issues.
```

**9-category checklist**:
| Category | What It Catches |
|---|---|
| Dead Code | Unused imports, functions, classes |
| Duplication | Copy-pasted logic, repeated patterns |
| God Files / Bloat | 300+ line files, mixed responsibilities |
| Over-Engineering | Unnecessary abstractions, future-proofing |
| Pydantic / Dataclass | Redundant models, missing validators |
| Error Handling | Swallowed exceptions, inconsistent strategies |
| Code Hygiene | Magic numbers, f-strings in logging |
| Hardcoded Values | Inline prompts, model names, URLs |
| Agent Architecture | Keyword routing, rigid pipelines, coupled prompts |

Uses ReACT reasoning internally to verify each finding. Output: structured Markdown report with severity (Critical/Warning/Info), evidence, impact, fix plans.

## God File Decomposer

When any file exceeds 300 lines, run:
```
Decompose app/agent.py — it's grown too large.
```

Analyzes internal dependencies, coupling types (data/stamp/control/content/common), proposes safe splits with circular dependency pre-analysis.

**Typical `agent.py` decomposition**:
| Before | After | Responsibility |
|---|---|---|
| `agent.py` (800+ lines) | `agent.py` | Orchestration only |
| | `core/intent_classifier.py` | Intent classification |
| | `core/skill_loader.py` | Skill lifecycle |
| | `core/prompt_builder.py` | System prompt construction |
| | `providers/odata_provider.py` | Data access abstraction |
| | `guardrails/enforcer.py` | Guardrail enforcement |

Produces Decomposition Manifest: exact files to create, exact line ranges to move, `__init__.py` re-exports for import preservation, step-by-step migration, risk assessment.

## Anti-Patterns Quick Reference

| Anti-Pattern | Fix |
|---|---|
| `if "create" in query:` | LLM-based intent classification (`result_type=IntentClassification`) |
| 100-line `_get_system_prompt()` | Externalize to `prompts/system_prompt.yaml` + `PromptBuilder` |
| `LiteLLMModel('sap/...')` scattered in 5 files | Centralize in config; `build_router()` factory |
| No max_iterations | Add `LoopController` with `max_iterations=10`, `max_llm_calls=10` |
| `except Exception: pass` | Exception hierarchy with `ErrorCategory` + severity + recoverability |
| Duplicated validation logic | Extract to shared skill with SKILL.md |
| No guardrails | `guardrails/config.yaml` with blocked intents and hard limits |

## When to Review

| Trigger | Action |
|---|---|
| After initial vibe coding session | Vibe Code Reviewer on `app/` |
| Before every PR | Vibe Code Reviewer (full) |
| Any file exceeds 300 lines | God File Decomposer on that file |
| Before production deploy | Both prompts |

**Minimum viable**: run Vibe Code Reviewer before every PR. Takes 2-5 minutes.

Both tools are **read-only** — they produce reports but never modify files.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Prompt_Engineering]] — externalized prompts
- [[SAP_Agent_Guardrails]] — adding guardrails
- [[SAP_Agent_Error_Handling]] — proper exception hierarchy
- [[SAP_Agent_Skills]] — extracting duplicated logic into skills
- [[AI_Team_Coding_Practice]] — complementary code quality system: SAP Vibe Code Reviewer (LLM-driven audit) + AGENTS.md/DECISIONS.md (context assets) are two sides of the same quality discipline

[Source: raw/SAP/code-quality-deep-dive.md]
