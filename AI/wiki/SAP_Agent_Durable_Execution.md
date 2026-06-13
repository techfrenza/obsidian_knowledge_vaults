---
title: SAP Agent Durable Execution
aliases: ["durable workflow", "SAP LangGraph persistence"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, durable-execution, LangGraph, Temporal, checkpointing]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Durable Execution

Stateless request-response agents fail for enterprise workflows. Durable execution persists full state after every step — pod crashes, restarts, and multi-day HITL waits become transparent.

## When You Need It

| Scenario | Problem Without Durability |
|---|---|
| Human approval arriving tomorrow | Pod killed after 30s timeout — state lost |
| Multi-step transaction (S/4HANA + CRM + logistics) | Pod crash after step 2 = step 1 ran, step 3 never executes |
| 50 sequential LLM calls | OOM kill at call 40 — restart burns duplicate LLM costs |
| Polling external API hourly | Cannot express as single HTTP request |

**Don't need it**: simple, fast, single-session agents. Durability adds infrastructure complexity.

## Decision Guide

Use durable execution if your agent:
- Runs for **>30 seconds** (HTTP timeout risk)
- Requires **HITL** (approval that may come hours/days later)
- Executes **critical transactions** (payments, financial postings) where double-execution causes damage
- Orchestrates **5+ dependent steps** across services

## Framework Options

| Framework | Approach | SAP Managed? | When |
|---|---|---|---|
| **LangGraph** | Graph state → Postgres checkpoints | Via AppFND | **Start here** |
| **Temporal** | Server + worker; industry standard | ✅ SAP-managed | Cross-service transactions, days-long workflows |
| **DBOS** | Library-based, requires PostgreSQL | Self-managed | Alternative to Temporal |
| **Restate** | Single binary; BSL license | Self-managed | Niche use cases |
| **Dapr Agents** | CNCF sidecar | Self-managed | Kubernetes-native deployments |

**Default**: LangGraph — built into AppFND, already in SDK, handles most requirements.
**Escalate to Temporal**: cross-system orchestration spanning multiple services, very long workflows (days/weeks), strong transactional guarantees.

## LangGraph Durability (Primary Path)

```python
checkpointer = PostgresSaver.from_conn_string(DATABASE_URL)
graph = build_agent_graph().compile(checkpointer=checkpointer)

# A2A context_id → LangGraph thread_id
config = {"configurable": {"thread_id": context_id}}
result = await graph.ainvoke(input, config=config)
```

HITL pause/resume via `interrupt()` in any node — state persisted, pod restarts transparent.

## Temporal (Advanced)

```python
@activity.defn
async def run_agent_step(task_input: dict) -> dict:
    agent = Agent(model="sap/anthropic--claude-4.5-sonnet", ...)
    result = await agent.run(task_input["message"])
    return {"output": result.data}

@workflow.defn
class FinanceAgentWorkflow:
    @workflow.run
    async def run(self, request: dict) -> dict:
        step1 = await workflow.execute_activity(run_agent_step, request)
        approval = await workflow.wait_for_signal("human_approval")  # safe across restarts
        if approval["approved"]:
            return await workflow.execute_activity(execute_transaction, step1)
```

SAP-managed Temporal: see [SAP Temporal Onboarding](https://pages.github.tools.sap/temporal/onboarding/).

## Three Design Patterns

### Long Research Task
```
[Plan] → [Search 1] → ... → [Search N] → [Synthesize] → [Human Review] → [Final Report]
         checkpoint          checkpoint    checkpoint      checkpoint (pause)
```
Pod crash at search 40 → resumes at search 41, no repeated work.

### Critical Transaction
```
[Validate] → [Create S/4HANA Order] → [Update CRM] → [Send Notification]
              checkpoint               checkpoint       checkpoint
```
Crash after step 2: framework sees checkpoint, skips completed step, continues to step 3. No duplicate orders.

### HITL Approval
```
[Prepare Proposal] → [PAUSE: Wait for Manager] → [Execute if Approved]
                     state persisted, can wait days
```

## Memory Service vs Durable Execution

| | Durable Execution | Memory Service |
|---|---|---|
| Purpose | Recover from failures *within* a run | Persist knowledge *across* runs |
| Scope | One workflow instance | All runs, all users |
| Storage | Postgres / Temporal | SAP HANA Cloud |

See [[SAP_Agent_Memory_Service]] for cross-session persistence.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_LangGraph]] — LangGraph implementation details
- [[SAP_Agent_Memory_Service]] — cross-session knowledge persistence
- [[SAP_Agent_Error_Handling]] — checkpoint recovery
- [[Human_In_The_Loop]] — HITL design patterns

[Source: raw/SAP/durable-agents-deep-dive.md]
