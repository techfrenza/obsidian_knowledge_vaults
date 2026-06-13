---
title: SAP Agent Error Handling
aliases: ["SAP exception hierarchy", "agent error recovery"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, error-handling, exceptions, retry, dead-letter-queue]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Error Handling

Structured exception hierarchy, loop control, retry policies, and dead-letter queues for enterprise SAP agents.

## Exception Hierarchy (`errors/exceptions.py`)

```
AgentError (base)
├── UserInputError      (USER_INPUT category)
├── ValidationError     (VALIDATION category)
├── BackendError        (BACKEND category — retryable)
├── NetworkError        (NETWORK category — retryable)
├── TimeoutError        (TIMEOUT category — retryable)
├── GuardrailError      (GUARDRAIL category — not retryable)
├── MaxIterationsError  (RESOURCE category)
└── MaxTimeError        (RESOURCE category)
```

`ErrorCategory` enum: USER_INPUT, VALIDATION, BACKEND, NETWORK, TIMEOUT, GUARDRAIL, RESOURCE, INTERNAL.
`ErrorSeverity` enum maps to response icons: ❌ critical, 🚨 high, ⚠️ warning, ℹ️ info.
`AgentError.to_dict()` for structured logging.

## AgentLoopController (`core/loop_controller.py`)

```python
@dataclass
class LoopConfig:
    max_iterations: int = 10    # total agent loop iterations
    max_time_seconds: int = 300  # 5-minute hard wall clock limit
    max_llm_calls: int = 10     # LLM API calls per request
    max_odata_calls: int = 50   # OData reads/writes per request
```

`LoopState` enum: RUNNING / COMPLETED / WAITING_INPUT / ERROR / MAX_ITERATIONS / MAX_TIME / CANCELLED.
`LoopMetrics`: tracks counts for each resource type.

`AgentLoop.run()`: `finally: await self._cleanup()` — always runs `ResourceManager.cleanup()` on exit regardless of success/failure.

## RetryPolicy

**Retryable status codes**: 408, 429, 500, 502, 503, 504
**Non-retryable**: 400, 401, 403, 404, 422

`@with_retry(max_retries=3)` decorator: exponential backoff with jitter. `get_backoff_delay(attempt)` formula: `min(2^attempt + random_jitter, max_delay)`.

## CheckpointManager

Save/load/delete checkpoints keyed by `context_id`. `recover_or_start(context_id, query)` — resume interrupted runs without re-executing completed steps.

## DeadLetterQueue

Failed operations that exhausted retry budget:
```python
dlq.add(operation)                             # enqueue failed op
await dlq.process_retry(processor, batch_size) # drain and retry
```
Separate retry queue prevents re-processing items already being drained. Wired as ASGI lifespan background task OR pod startup hook — ensures failed ops eventually complete.

## ResourceManager

Register cleanup functions with priority integers. `cleanup(context_id)` executes all registered functions in priority order — handles partial failures, connection pool release, temp file deletion.

## ErrorResponseBuilder

Maps `ErrorSeverity` → icon + title + user message. Finance-specific templates:
- `FISCAL_PERIOD_CLOSED` — posting to closed fiscal period
- `DEBIT_CREDIT_IMBALANCE` — journal entry doesn't balance
- `GL_ACCOUNT_NOT_FOUND` — GL account not in company code
- `UNAUTHORIZED` — role authorization limit exceeded

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Resilience]] — circuit breaker and retry at infrastructure level
- [[SAP_Agent_Durable_Execution]] — checkpoint recovery for long-running agents
- [[SAP_Agent_Output_Validation]] — validation before write operations
- [[SAP_Agent_Testing]] — testing error handling paths with TestModel

[Source: raw/SAP/error-handling-deep-dive.md]
