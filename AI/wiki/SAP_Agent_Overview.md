---
title: SAP Agent Engineering Overview
aliases: ["SAP Agent MOC", "SAP PydanticAI", "AppFND agents"]
parent: "[[index]]"
tags: [SAP, agent-engineering, PydanticAI, LiteLLM, AppFND, A2A]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Engineering Overview

Central map-of-content for the SAP enterprise agent stack. All agents are built on PydanticAI + LiteLLM running on AppFND (Application Foundation), communicate over the A2A protocol, and access SAP backends via OData through MCP servers.

## Stack Summary

| Layer | Technology |
|---|---|
| Framework | [[PydanticAI]] + [[LangGraph_Build_Agents|LangGraph]] (both first-class in AppFND SDK) |
| LLM routing | LiteLLM with `sap/` prefix via [[SAP_Agent_Performance|SAP Hyperspace AI Proxy]] |
| Primary model | `sap/anthropic--claude-4.5-sonnet` (capable tier) |
| Inter-agent comms | [[SAP_Agent_Multi_Agent|A2A Protocol]] (JSON-RPC 2.0, `tasks/send`) |
| Tool access | [[SAP_Agent_MCP_Integration|MCP Servers]] → BTP Destination → S/4HANA OData |
| Deployment | AppFND; agent discoverable at `/.well-known/agent.json` |
| Boilerplate | `sap-agent-bootstrap` skill generates: `main.py`, `agent.py`, `agent_executor.py`, `Dockerfile`, `app.yaml`, `requirements.txt` |

## Key Sub-topics

- [[SAP_Agent_Prompt_Engineering]] — Layered system prompt, externalized YAML, structured output, few-shot, hallucination mitigation
- [[SAP_Agent_Multi_Agent]] — A2AClient, Sequential/Parallel/Routing patterns, AgentContext, FinanceOrchestrator
- [[SAP_Agent_MCP_Integration]] — MCP server as OData abstraction, SemanticFieldSelector, FieldMapper, DestinationServiceClient
- [[SAP_Agent_Guardrails]] — 6-layer defense, YAML config, GuardedMCPToolset, OutputValidator
- [[SAP_Agent_Guardrails_MCP]] — GuardedMCPToolset middleware: per-agent rule injection at agent side, EnforceableRule interface, AmountLimitRule
- [[SAP_Agent_Resilience]] — CircuitBreaker, LiteLLM Router, write-agent safety, Bulkhead, layered timeouts
- [[SAP_Agent_Error_Handling]] — Exception hierarchy, AgentLoopController, DeadLetterQueue, RetryPolicy
- [[SAP_Agent_Output_Validation]] — Three-Verdict Pattern, Single-Execution Guard, LangGraph placement
- [[SAP_Agent_Testing]] — Testing pyramid (5 layers), PydanticAI TestModel, Aeval framework
- [[SAP_Agent_Performance]] — Batching, TieredPromptManager, MultiLayerCache, ParallelFetcher
- [[SAP_Agent_Skills]] — SKILL.md format, SkillLifecycleManager, activation models
- [[SAP_Agent_Cards]] — Agent Card JSON schema, AgentRegistry, naming convention `pc-{domain}-{function}-agent`
- [[SAP_Agent_LangGraph]] — LangGraph nodes/state/edges, HITL, PostgresSaver checkpointing
- [[SAP_Agent_Durable_Execution]] — LangGraph checkpoints vs Temporal vs DBOS; decision guide
- [[SAP_Agent_Memory_Service]] — Episodic/Semantic/Procedural memory on SAP HANA Cloud
- [[SAP_Agent_ORD_Registration]] — ORD endpoint, UMS, UCL, TR6 requirement
- [[SAP_Agent_UMS_Registry]] — Unified Metadata Service, system-version vs system-instance
- [[SAP_Agent_Joule_Integration]] — Agent Gateway, IAS App2App, Joule design-time artifacts
- [[SAP_Agent_Ship_Checklist]] — TR1–TR14 technical requirements, metering, Agent Steps
- [[SAP_Agent_Code_Quality]] — Vibe Code Reviewer, God File Decomposer, anti-patterns
- [[SAP_Agent_Evaluation]] — Testing Onion (3 layers), constrained agency, aeval framework

## 13-Step Production Path

1. Bootstrap with `sap-agent-bootstrap`
2. Add intent classification (PydanticAI `result_type=IntentClassification`)
3. Replace flat prompt with `PromptBuilder` + YAML layers
4. Add OData provider via MCP server + `DestinationServiceClient`
5. Add guardrails (YAML config + `GuardedMCPToolset`)
6. Add skills (SKILL.md format + `SkillLifecycleManager`)
7. Add resilience (CircuitBreaker + LiteLLM Router)
8. Add output validation (`OutputValidator` + Single-Execution Guard)
9. Add error handling (exception hierarchy + `AgentLoopController`)
10. Add observability (`auto_instrument()` + OTel)
11. Code review (Vibe Code Reviewer + God File Decomposer)
12. Testing (unit → behavioral → E2E → aeval)
13. Ship readiness (TR1–TR14 checklist, ORD endpoint, Joule integration)

[Source: raw/SAP/from-boilerplate-to-production.md]
