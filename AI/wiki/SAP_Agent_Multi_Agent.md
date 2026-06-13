---
title: SAP Agent Multi-Agent Patterns
aliases: ["SAP multi-agent", "A2A orchestration"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, multi-agent, A2A, orchestration, PydanticAI]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Multi-Agent Patterns

A2A (Agent-to-Agent) protocol is Google's open JSON-RPC 2.0 standard. SAP agents communicate via `tasks/send` method; agents are discoverable at `/.well-known/agent.json`.

## A2AClient

```python
class A2AClient:
    async def send_task(self, message: str, data: Optional[dict] = None,
                        session_id: Optional[str] = None, task_id: Optional[str] = None) -> A2AResponse
    async def fetch_agent_card(self) -> dict
```

Located at `core/a2a_client.py`. Data types: `A2ATask`, `A2AResponse`, `A2AClientError`.

## AgentContext — Standard Envelope

Forwarded between ALL agents via A2A data parts (`to_a2a_data()` / `from_a2a_data()`):

```
company_code | fiscal_year | document_refs | requestor_role | correlation_id | chain
```

`forward(current_agent)` appends current agent name to chain — creates an audit trail across the entire call tree.

## Four Orchestration Patterns

### 1. Sequential Pipeline (`orchestration/sequential.py`)
```python
run_sequential_pipeline(steps, initial_data, session_id)
```
Each step receives output of prior step. Use for: data validation → enrichment → posting.

### 2. Parallel Fan-Out/Fan-In (`orchestration/parallel.py`)
```python
fan_out(targets, timeout_seconds)  # asyncio.gather
```
All agents run concurrently; results aggregated. Use for: multi-ledger analysis, parallel document fetch.
**Error rule**: outer orchestrator timeout must exceed sum of inner agent timeouts.

### 3. IntentRouter — Conditional Routing (`orchestration/routing.py`)
PydanticAI structured-output classifier → `AgentRoute` map → delegates via A2A. Replaces brittle keyword routing (`if "create" in query`).

### 4. FinanceOrchestrator — Domain Orchestrator (`orchestrator/finance_orchestrator.py`)
- Handles `GENERAL_INQUIRY` locally (no delegation)
- Routes specialized intents (journal entry, clearing, accruals) to specialist agents
- Returns `input_required` state when user confirmation needed
- `StatefulFinanceOrchestrator` resumes pending confirmations from prior conversation turns

## AgentRegistry

Central registry indexed by domain tag:
```python
registry.register(url)              # fetches /.well-known/agent.json
registry.find_by_domain("finance")  # returns all finance agents
registry.find_by_intent("CREATE_JOURNAL_ENTRY")
registry.health_check_all()
```

`AgentRegistryClient.get_client_for_agent(name)` returns a pre-configured `A2AClient`.

## SessionStore — Multi-Turn State

In-memory with Redis backing in production. TTL=3600s. `ConversationTurn` records. `get_history_summary(max_turns=5)` for context injection.

## Human-in-the-Loop (HITL)

Agent returns `input_required` state when `ConfirmationRule` is triggered:
- `high_value_posting` — amount > 10,000
- `cross_company` — posting spans multiple company codes
- `clearing_execution` — executing an AP clearing run

HITL message format: proposed action summary + which rule fired + plain-English reasoning + approve/reject/correct options.

## Error Propagation Rules

- Never auto-retry non-idempotent operations (journal entry creation, clearing execution)
- Partial failures in Fan-Out are reported individually, not as aggregate failure
- Outer orchestrator timeout > sum of all inner agent timeouts

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Cards]] — Agent Card schema for discovery
- [[SAP_Agent_Error_Handling]] — error propagation hierarchy
- [[SAP_Agent_LangGraph]] — LangGraph sub-graph delegation alternative
- [[Multi_Agent_Architecture]] — general multi-agent patterns
- [[LangGraph_Deep_Agents]] — stateful multi-agent patterns; SAP uses A2A protocol where LangGraph uses Command/sub-graphs — different implementations of the same orchestration needs
- [[Human_In_The_Loop]] — HITL design patterns
- [[Agentic_Loop]] — loop control patterns

[Source: raw/SAP/multi-agent-deep-dive.md]
