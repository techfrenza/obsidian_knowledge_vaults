---
title: SAP Agent Prompt Engineering
aliases: ["SAP prompts", "SAP system prompt"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, prompt-engineering, PydanticAI, LiteLLM, enterprise-AI]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Prompt Engineering

Patterns for effective prompt design in PydanticAI + LiteLLM agents on AppFND. The boilerplate gives a flat `_get_system_prompt()` string; this replaces it with a maintainable, externalized, version-controlled system.

## Layered System Prompt Architecture

Build from 7 structured layers via `PromptBuilder` (`app/core/prompt_builder.py`):

| Layer | XML Tag | Content |
|---|---|---|
| 1 | `<role>` | Agent identity and capability |
| 2 | `<constraints>` | Hard behavioral rules |
| 3 | `<domain_knowledge>` | SAP business context (doc types, field meanings) |
| 4 | `<available_skills>` | Injected dynamically from SkillLoader |
| 5 | `<guardrails>` | Injected from Guardrails config |
| 6 | `<output_format>` | Response format instructions |
| 7 | `<context>` | Runtime: user role, company code, fiscal year |

Config externalized to `prompts/system_prompt.yaml` — version-controlled, non-dev editable.

## Context Window Management

Three strategies for large enterprise data:
- **Summarize**: `summarize_large_dataset(data, max_items=20)` — show first/last 10 + count for datasets >20 rows
- **Sliding window**: `truncate_conversation(messages, max_messages=20, always_keep_first=True)` — keeps setup context + recents
- **Progressive detail**: metadata only (>100 items) → sample + stats (>20) → full data (<20)

Token estimation: `len(text) // 4` (rough heuristic). Reserve 20% of context window for output. For safety-critical decisions (pre-write validation), use actual tokenizer or larger safety margin.

## Model Context Limits

| Model | Context |
|---|---|
| `sap/anthropic--claude-4.6-sonnet` | 200K |
| `sap/gpt-5` | 128K |
| `sap/gemini-2.5-pro` | 1M |

## Structured Output with PydanticAI

`result_type` forces the LLM to return a validated Pydantic model — eliminates parsing errors.

```python
journal_agent = Agent(
    model=LiteLLMModel('sap/anthropic--claude-4.5-sonnet'),
    result_type=JournalEntryProposal,  # Guaranteed type-safe output
    system_prompt="..."
)
result = await journal_agent.run("Create journal entry for office supplies, $500, company 1010")
proposal: JournalEntryProposal = result.output  # typed, validated
```

Key Pydantic models: `JournalEntryProposal` (header + line items + balance check), `IntentClassification` (intent, confidence, entities, requires_confirmation).

## Tool Use Patterns

`@agent.tool` decorator exposes Python async functions. Docstrings and type hints are the tool schema for the LLM.

**Tool vs inline knowledge decision:**
- Use tools: data changes frequently, user-system-specific, side effects, large volumes
- Use inline: static data (doc type codes), universal SAP field definitions, pure computation

## Few-Shot Examples

YAML example banks in `prompts/few_shot/`. Load with `load_examples(path, max_examples=3)` → inject as `<examples>` XML in prompt. For domain-specific finance tasks, few-shot examples produce 2-5× accuracy improvement.

## Hallucination Mitigation

Critical for SAP finance — a hallucinated GL account or amount has real financial impact:
1. **Ground first**: fetch real SAP data via OData, THEN ask LLM to select from it
2. **Validate after**: `validate_proposal()` checks LLM output against live SAP (`GLAccountSet`, balance check)
3. **Confidence signals**: prompt LLM to express uncertainty ("I suggest GL XXXX but please verify") rather than guessing

## Model Selection

```
Write to SAP? → Capable tier (Sonnet/GPT-5) — NO fast-tier fallback for writes
Classification/Validation/Extraction? → Fast tier (Haiku/GPT-5-mini)
```

**Capable tier** (complex reasoning, tool use):
- `sap/anthropic--claude-4.6-sonnet` — best tool use + structured output
- `sap/gpt-5` — 60% cheaper than Sonnet input cost; strong cross-provider fallback
- `sap/gemini-2.5-pro` — 1M context; third-provider fallback

**Fast tier** (classification, extraction, validation):
- `sap/anthropic--claude-4.5-haiku` — best fast structured output
- `sap/gpt-5-mini` — 70% cheaper than Haiku
- `sap/gpt-5-nano` — cheapest; simple routing only

Cost insight: cross-provider fallback to GPT-5 saves 60% AND provides resilience — fallback is a cost play, not just reliability.

## Prompt Versioning

Prompts live in git — treat as code. SHA-256 hash of system prompt for regression detection. Use `TestModel` from PydanticAI for prompt regression tests (no API calls, deterministic).

## Key Files

```
prompts/
├── system_prompt.yaml        # Main prompt layers
├── intent_classifier.yaml    # Intent classification prompt
├── skills/                   # Skill-specific overrides
└── few_shot/                 # Example banks
```

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Resilience]] — LiteLLM Router configuration for model fallback
- [[SAP_Agent_Skills]] — skill injection into Layer 4
- [[SAP_Agent_Guardrails]] — guardrail injection into Layer 5
- [[SAP_Agent_Testing]] — TestModel for prompt regression testing
- [[Prompt_Engineering_Advanced]] — general advanced patterns
- [[Context_Engineering]] — context window management theory

[Source: raw/SAP/prompt-engineering-deep-dive.md]
