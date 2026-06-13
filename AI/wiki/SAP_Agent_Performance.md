---
title: SAP Agent Performance
aliases: ["SAP agent performance", "TieredPromptManager"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, performance, caching, batching, token-optimization]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Performance

5 performance priorities for SAP agents, in order: (1) minimize LLM calls via batching, (2) reduce prompt size via tiered loading, (3) parallelize I/O, (4) cache aggressively, (5) stream responses.

## Priority 1: Minimize LLM Calls (Batching)

`BatchingStrategy.should_batch()`: combine 3 sequential LLM analyses into 1 call if `combined_tokens < 8000`. Single batched call vs 3 sequential calls: ~3× latency reduction, ~3× cost reduction.

## Priority 2: Reduce Prompt Size (Tiered Loading)

`TieredPromptManager`:
| Tier | Tokens | When Loaded |
|---|---|---|
| L1 Core | ~500 | Always — role, constraints |
| L2 Domain | ~1000 | Always — SAP-specific knowledge |
| L3 Skill | ~2000 | On skill activation |
| L4 Examples | ~1000 | On demand only |

`PromptCompressor`: summarize datasets >50 items (show 10 samples + stats); remove example lines if over token budget.
`AdaptivePromptSelector`: assess query complexity (simple/standard/complex) → select 500/2000/4000 token prompt.

`SmartRouter`: pattern-match common queries before invoking LLM:
- `^show document (\d+)$` → direct OData call, zero LLM cost
- `^list (journal entries|documents)$` → parametrized OData
- `^help$` → static response

## Priority 3: Parallelize I/O

`ParallelFetcher`: `asyncio.Semaphore(max_concurrent)` + `asyncio.gather(return_exceptions=True)`. Use for: fetching multiple GL account details, parallel OData lookups across ledgers.

`ConnectionPool`: singleton `httpx.AsyncClient` — `max_connections=100`, `max_keepalive_connections=20`, `http2=True`. Eliminates connection setup overhead for OData calls.

## Priority 4: Cache Aggressively

`MultiLayerCache`: L1 (in-memory dict, LRU eviction at 1000 entries) + L2 (Redis).
```python
cache.set(key, value, l1_ttl=60, l2_ttl=3600)
```

Cache TTL policy by entity type:
| Entity | TTL | Rationale |
|---|---|---|
| GLAccount | 3600s | Master data, rarely changes |
| CostCenter | 3600s | Master data |
| ExchangeRate | 900s | Updates periodically |
| JournalEntry | 60s | Recent transactions |
| AccountBalance | 0 | Never cache — always real-time |

## Priority 5: Stream Responses

Use A2A `tasks/sendSubscribe` for streaming. Users see first token ~200ms vs waiting for complete response (~3-5s). Critical for user perception of agent speed even when total time is the same.

## Observability

`PerformanceMetrics`: `operation_times` dict → `get_summary()` with avg/max/min/p95 per operation type.
`RequestProfiler`: visual timeline with `█` bar chart for each processing phase → identifies bottleneck phase instantly.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Skills]] — tiered skill loading (同构设计：SkillLifecycleManager discover→activate ≡ TieredPromptManager L1→L4 按需加载)
- [[SAP_Agent_Prompt_Engineering]] — context window management strategies
- [[SAP_Agent_MCP_Integration]] — OData call optimization
- [[SAP_Agent_Resilience]] — timeout layering
- [[Tokenmaxxing]] — token optimization theory

[Source: raw/SAP/performance-deep-dive.md]
