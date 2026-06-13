---
title: SAP Agent Evaluation
aliases: ["SAP agent eval", "constrained agency"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, evaluation, aeval, constrained-agency, quality]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Evaluation

Structured agent evaluation framework. Testing catches regressions; evaluation measures quality — they're different activities with different cadences.

## Testing vs Evaluation

| | Testing | Evaluation |
|---|---|---|
| Purpose | Identify defects, verify expected behavior | Compare versions, assess overall quality |
| Scope | Functionality, error handling, robustness | Effectiveness, coherence, interaction quality |
| Frequency | Ongoing throughout development | At specific milestones (releases, model upgrades) |

## Constrained Agency Philosophy

LLMs are non-deterministic. For enterprise agents, **explicitly constrain autonomy** rather than relying on the model to infer correct behavior.

**Bad (too open)**:
```
Use the available tools to help the user clear open items.
```

**Good (constrained)**:
```
STEP 1: Extract company code, fiscal year, clearing date from request.
STEP 2: Call get_open_items with company code and fiscal year.
STEP 3: Present items and ask for confirmation.
STEP 4: Call execute_clearing with confirmed items only.
```

Explicit step instructions = more deterministic, testable behavior. Limit tools per task, constrain parameter ranges, define decision rules. More autonomy is the long-term goal — earned incrementally through validated production behavior.

## Testing Onion (3 Layers)

```
┌─────────────────────────────────────┐
│    Production Integration Tests     │  ← E2E with real tools and Joule
│  ┌───────────────────────────────┐  │
│  │    Agent Behavior Assessment  │  │  ← Agent reasoning, tool selection (mocked tools)
│  │  ┌─────────────────────────┐  │  │
│  │  │   Tool Functionality    │  │  │  ← Isolated tool/MCP unit tests
│  │  └─────────────────────────┘  │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

**Layer 1 (Tool Functionality)**: test OData wrappers, MCP tools in isolation. Verify auth, error handling, pagination, response schemas. Tools not tested in isolation have no business being handed to an LLM.

**Layer 2 (Agent Behavior)**: mocked tools, verify tool selection, parameters, error handling, guardrails, response quality.

**Layer 3 (Production Integration)**: real tools + live Joule/Agent Gateway path. Use as smoke tests only.

## Test Dimensions and Priority

| Dimension | Priority |
|---|---|
| Correctness — final responses | High |
| Correctness — tool calls | High |
| Hallucinations | High |
| Summarization quality | High — requires LLM-as-judge |
| Scenario selection (Joule) | High — Joule CLI tests |
| Memory / context retention | Medium |
| Safety guardrails | Medium |
| Security / auth | Medium |

## Aeval Framework

SAP's standard automated evaluation tool. Skills: `aeval-set-up` + `aeval-run-eval`.

**Prerequisite**: OTel telemetry instrumentation active — aeval reads trace data.

Setup:
```
/aeval-set-up   # configure MLflow integration and A2A evaluation server
/aeval-run-eval # run evaluations against your agent
```

Metrics: task completion rate, tool call accuracy (from trace), response quality, latency, consistency (variance across repeated runs).

## Best Practices

- **Run 10× minimum**: single-run pass insufficient for non-deterministic models
- **Mock tools for behavior tests**: real backends slow down regression suites
- **LLM-as-judge for semantics only**: regex/JSONPath for deterministic checks
- **Real tools for smoke tests**: catches auth issues, routing failures, backend schema changes

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Testing]] — testing pyramid (5 layers), TestModel, Aeval YAML format
- [[SAP_Agent_Prompt_Engineering]] — prompt versioning, regression testing
- [[SAP_Agent_Code_Quality]] — pre-PR code review as quality gate
- [[Unique_Engineering_Insights]] — "Constrained Agency" aligns with "Harness > Model" insight: explicit step instructions = deterministic harness overriding emergent model behavior

[Source: raw/SAP/evaluation-deep-dive.md]
