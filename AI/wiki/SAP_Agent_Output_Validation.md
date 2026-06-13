---
title: SAP Agent Output Validation
aliases: ["PydanticAI validation", "SAP output schema"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, output-validation, LLM-as-judge, write-safety, PydanticAI]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Output Validation

Semantic validation before SAP write operations. Pydantic validates data shape; `OutputValidator` validates business meaning — two different things.

## When Semantic Validation Is Required

Any operation that modifies SAP data: CREATE, UPDATE, DELETE/REVERSE, POST. Read-only operations do not require semantic validation.

## Three-Verdict Pattern

`ValidationResult` fields: `rule_id`, `verdict` (PASS / FAIL / WARNING), `reason`, `confidence`.

`ValidationReport`: `.passed` (all PASS), `.needs_review` (at least one WARNING), `.failures()`, `.warnings()`. Verdict selection: **most severe wins** — if any rule returns FAIL, overall = FAIL.

**Severity cap**: a `severity: WARNING` rule CAN return WARNING but NEVER FAIL — the cap is applied after the LLM response.

## OutputValidator (`app/validation/output_validator.py`)

```python
class OutputValidator:
    def __init__(self, rules_path: Path, model: str = "sap/anthropic--claude-haiku-3")
    async def validate(self, context: dict) -> ValidationReport
    async def _evaluate_rule(self, rule: dict, context: dict) -> ValidationResult
```

All rules run concurrently (`asyncio.gather`). A crashed rule evaluation → automatic FAIL verdict.

**Finance validation rules** (`rules.yaml`):
| Rule ID | Verdict if Failed | What It Checks |
|---|---|---|
| `GL_ACCOUNT_INTENT_MATCH` | FAIL | GL account matches user's stated intent |
| `FISCAL_PERIOD_OPEN` | FAIL | Target fiscal period is open for posting |
| `AMOUNT_AUTHORITY` | WARNING | Amount within user's authorization level |
| `DUPLICATE_DETECTION` | WARNING | Likely duplicate of recent posting |

## Single-Execution Guard (`app/validation/guard.py`)

Prevents duplicate write-tool calls within one agent run — critical for idempotency.

```python
WRITE_TOOLS = frozenset({"post_journal_entry", "clear_ap_items",
                          "create_billing_document", "reverse_posting", "post_goods_receipt"})

def guarded_tool_call(state: AgentState, tool_name: str, tool_fn, **kwargs)
```

`_called_write_tools` is stored in LangGraph `AgentState` — survives checkpoint recovery. If the agent crashes after posting but before acknowledging, the guard prevents double-posting on resume.

## LangGraph Placement

Dedicated `validate_output_node` → conditional edge `route_on_verdict`:
- PASS → `execute_write`
- WARNING → `await_human` (HITL confirmation)
- FAIL → `abort` (return error to user)

## PydanticAI Placement

Wrap write function: `validated_post_journal_entry()`. Raises `ValueError` on FAIL. Returns `"VALIDATION_WARNING: ..."` string on WARNING (agent surfaces this to user). PASS continues normally.

## HITL Message Format

When WARNING triggers human review:
1. Proposed action summary
2. Which rule fired and why
3. Plain-English explanation of the concern
4. Options: approve / reject / correct

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Guardrails]] — 6-layer defense (output validation is Layer 5)
- [[SAP_Agent_LangGraph]] — graph node placement
- [[SAP_Agent_Multi_Agent]] — HITL workflow
- [[SAP_Agent_Resilience]] — write-agent model fallback safety
- [[Human_In_The_Loop]] — general HITL patterns

[Source: raw/SAP/output-validation-deep-dive.md]
