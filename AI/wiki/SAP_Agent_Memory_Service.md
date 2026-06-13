---
title: SAP Agent Memory Service
aliases: ["SAP memory", "episodic semantic procedural"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, memory, HANA-cloud, cross-session, personalization]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Memory Service

Persistent memory layer backed by SAP HANA Cloud. Without it, agents are stateless — every run starts from zero. Memory Service enables cross-session continuity, personalization, and proactive intelligence.

> **Status**: Under active development as of 2026-05. APIs and integration patterns being finalized. Contact Christian Schuetz or Shabana Samsudheen for current availability.

## Three Memory Types

| Type | Stores | Example |
|---|---|---|
| **Episodic** | Past interactions, conversations, events | "Last week this user asked about clearing run #4471" |
| **Semantic** | Extracted facts, structured knowledge | "Company code 1000 has 30-day payment terms" |
| **Procedural** | Workflows, decision patterns, past actions | "When clearing fails, always check open credits first" |

Separation prevents conflating raw history (episodic) with derived understanding (semantic, procedural).

## Key Capabilities Enabled

- **Context continuity** — prior interactions preserved across sessions and pod restarts
- **Improved reasoning** — historical data informs deeper analysis (e.g., recurring AP exceptions)
- **Personalization** — responses shaped by past behavior and preferences
- **Proactive intelligence** — agent identifies trends, triggers recommendations without being asked
- **Efficiency** — retrieve only relevant facts instead of re-injecting full history

## When to Use

**Use memory when**:
- Multi-session workflows (clearing run spanning multiple days)
- Users expect prior context remembered ("run same clearing as last week")
- Agent should learn from repeated patterns (which exceptions are always overridden)
- Need to reduce prompt size by storing structured facts vs re-injecting full history

**Skip memory for**: single-session, stateless agents — adds complexity without benefit.

## Memory Service vs Durable Execution

| | Memory Service | Durable Execution |
|---|---|---|
| Scope | Cross-session knowledge | Within-run state recovery |
| Purpose | Remember what happened across runs | Resume exactly where crashed run stopped |
| Storage | SAP HANA Cloud | Workflow engine (Postgres / Temporal) |
| When needed | Multi-session continuity, personalization | Long-running tasks, HITL waits, critical transactions |

A production agent may use both: durable execution for reliability within a run, memory for continuity across runs.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Durable_Execution]] — within-run state recovery (complementary)
- [[SAP_Agent_LangGraph]] — LangGraph SessionStore (in-memory + Redis, single-session)
- [[Managed_Agent_Memory]] — general agent memory architecture
- [[Agentic_Memory_System]] — same Episodic/Semantic/Procedural three-type taxonomy used by SAP Memory Service
- [[Cross_Platform_Memory]] — cross-session memory patterns

[Source: raw/SAP/agent-memory-deep-dive.md]
