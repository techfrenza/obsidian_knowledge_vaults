---
title: SAP Agent LangGraph
aliases: ["SAP LangGraph", "AppFND LangGraph"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, LangGraph, stateful-agents, HITL, checkpointing]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent LangGraph

LangGraph is one of two frameworks with first-class AppFND SDK support (alongside PydanticAI). Choose LangGraph for complex multi-step workflows, stateful agents, HITL, and graph-based control flow.

## Core Concepts

| Concept | Description |
|---|---|
| **State** | TypedDict shared across all nodes — working memory for one run |
| **Node** | Python function that reads and updates state |
| **Edge** | Connection between nodes — can be conditional |
| **Graph** | Compiled workflow; `StateGraph → .compile()` |
| **Checkpointer** | Persists state to DB after each node — enables pause/resume |

## Standard State Definition

```python
from typing import TypedDict, Annotated
from langgraph.graph import add_messages

class AgentState(TypedDict):
    messages: Annotated[list, add_messages]  # Full conversation history
    task_id: str       # A2A task context
    context_id: str    # A2A context — used as thread_id for checkpointer
    intent: str | None
    tool_results: list
    _called_write_tools: set  # Single-Execution Guard (OutputValidator)
```

## AppFND Bootstrap Structure

```
app/
├── agent.py          # LangGraph graph definition
├── state.py          # AgentState TypedDict
├── nodes/
│   ├── intent.py     # Intent classification node
│   ├── tools.py      # Tool execution node
│   └── response.py   # Response formatting node
├── tools/
└── main.py           # A2A server + graph wiring
```

## Human-in-the-Loop (HITL)

```python
from langgraph.graph import interrupt

def approval_node(state: AgentState):
    human_response = interrupt({
        "message": f"Approve this action? {state['proposed_action']}",
        "proposed_action": state["proposed_action"]
    })
    # Graph PAUSES here — state persisted — resumes when user responds
    return {"approved": human_response["approved"]}
```

State is persisted at `interrupt()`. Pod restarts between pause and resume are transparent. Requires checkpointer.

## State Persistence (Checkpointers)

```python
# Production
from langgraph.checkpoint.postgres import PostgresSaver
checkpointer = PostgresSaver.from_conn_string(DATABASE_URL)

# Development
from langgraph.checkpoint.memory import MemorySaver
checkpointer = MemorySaver()

graph = build_graph().compile(checkpointer=checkpointer)

# A2A context_id maps to LangGraph thread_id — state isolated per conversation
config = {"configurable": {"thread_id": context_id}}
result = await graph.ainvoke(input_state, config=config)
```

## Multi-Agent with Sub-graphs

```python
from langgraph.types import Command

def orchestrator_node(state: AgentState):
    if state["intent"] == "clearing":
        return Command(goto="clearing_subgraph", update={"delegated": True})
    return Command(goto="general_response")

# Mount sub-graph as a node
parent_graph.add_node("clearing_subgraph", clearing_graph.compile())
```

## OutputValidator Integration in LangGraph

Dedicated `validate_output_node` → conditional edge `route_on_verdict`:
- PASS → `execute_write`
- WARNING → `await_human`
- FAIL → `abort`

## Observability

AppFND `auto_instrument()` wraps LangGraph execution with OTel spans automatically. Each node = one span; tool calls = child spans. No manual instrumentation needed for basic tracing.

## PydanticAI vs LangGraph Decision

| Use LangGraph When | Use PydanticAI When |
|---|---|
| Complex multi-step workflows with conditional branches | Simple single ReAct loop |
| HITL patterns (approval that arrives hours later) | Straightforward tool use |
| Graph-based control flow needed | Lightweight, less boilerplate |
| Durable execution required | Quick prototype |

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Durable_Execution]] — LangGraph vs Temporal decision guide
- [[SAP_Agent_Output_Validation]] — Single-Execution Guard in AgentState
- [[SAP_Agent_Multi_Agent]] — A2A delegation vs LangGraph sub-graphs
- [[LangGraph_Build_Agents]] — general LangGraph patterns
- [[LangGraph_Deep_Agents]] — advanced LangGraph patterns

[Source: raw/SAP/langgraph-deep-dive.md]
