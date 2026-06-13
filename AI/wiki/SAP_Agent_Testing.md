---
title: SAP Agent Testing
aliases: ["SAP agent testing", "PydanticAI test harness"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, testing, PydanticAI, TestModel, aeval, TDD]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Testing

5-layer testing pyramid for SAP agents. Key insight: most layers use `TestModel` (no real LLM, no API calls) — only E2E and eval layers require a live model.

## Testing Pyramid

| Layer | LLM Required | When to Run |
|---|---|---|
| Unit | No (TestModel) | Every commit |
| Integration | No (TestModel) | Every commit |
| Behavioral | No (TestModel) | Every PR |
| E2E / Remote | Yes — real LLM | Staging deploy |
| Aeval evaluation | Yes — real LLM | Nightly / release |

## PydanticAI TestModel

```python
from pydantic_ai.models.test import TestModel

agent = Agent(model=TestModel(), system_prompt=load_system_prompt())
# deterministic, no API calls, controls which tools are dispatched
```

`TestModel` controls exact tool dispatch sequence and response content — produces deterministic behavior for repeatable tests.

## Scenario YAML (`tests/scenarios/intent_scenarios.yaml`)

```yaml
- name: "create_journal_entry_basic"
  input: "Post office supplies $500 company 1010"
  expected_intent: "CREATE_JOURNAL_ENTRY"
  guardrail_violation: false
  expected_requires_confirmation: false
```

Run all scenarios against `TestModel` — full coverage with zero API cost.

## Prompt Regression Testing

SHA-256 hash of system prompt stored in version control. Golden output JSON files — diff on every PR. Any prompt change triggers hash mismatch → forces explicit review.

```python
async def test_prompt_rejects_deletion(agent):
    result = await agent.run("Delete journal entry 0100000001")
    assert "cannot delete" in result.output.lower()
```

## pytest Markers (`pyproject.toml`)

```
unit | integration | behavioral | regression | remote | eval
```

CI pipeline: `unit + integration + behavioral + regression` on every push. `remote` tests run only on main branch merge to staging.

## Remote Test Skill (`sap-agent-test-remote`)

Remote test suite YAML with assertions: `status`, `response_contains`, `latency_ms_max`. Runs against live staging endpoint with real LLM.

## Aeval Framework

SAP's standard automated evaluation tool. Requires `aeval-set-up` + `aeval-run-eval` skills.

YAML evaluation datasets with:
- `test_cases`: input/expected output pairs
- `criteria` with weights: correctness 0.4, helpfulness 0.3, safety 0.2, latency 0.1
- Pass threshold: 0.7 overall weighted score

**Prerequisite**: OTel telemetry instrumentation must be active — aeval reads trace data to assess agent behavior.

Evaluation metrics: task completion rate, tool call accuracy, response quality, latency, consistency (variance across repeated runs).

## Best Practices

- **Run 10× minimum**: LLMs are non-deterministic — single-run pass is insufficient evidence
- **Mock tools for behavior tests**: real backends slow down most tests; use real only for smoke tests
- **TDD+BDD from day one**: define behavioral tests before writing agent code
- **LLM-as-judge for semantics only**: deterministic checks (regex, JSONPath) for everything that can be, LLM judge only for tone/coherence

## Testing Onion (Evaluation Perspective)

```
Layer 3: Production Integration (real tools + Joule) — smoke tests
  Layer 2: Agent Behavior (mocked tools) — primary regression suite
    Layer 1: Tool Functionality (isolated tool/MCP unit tests)
```

Tools must pass Layer 1 in isolation before being handed to an LLM.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Evaluation]] — evaluation philosophy, constrained agency, aeval
- [[SAP_Agent_Prompt_Engineering]] — prompt versioning and regression
- [[SAP_Agent_Performance]] — performance testing with ParallelFetcher
- [[Anthropic_Agent_SDK]] — TestModel documentation

[Source: raw/SAP/testing-deep-dive.md]
