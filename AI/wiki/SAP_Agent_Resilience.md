---
title: SAP Agent Resilience
aliases: ["SAP resilience", "LiteLLM router", "circuit breaker"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, resilience, circuit-breaker, LiteLLM, fallback]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Resilience

Circuit breaking, LLM fallback routing, and resource isolation for production SAP agents. The write-agent safety constraint is the most critical: **write agents must NEVER fall back to fast-tier models**.

## CircuitBreaker (`core/circuit_breaker.py`)

```python
@dataclass
class CircuitBreakerConfig:
    failure_threshold: int = 5    # failures to open circuit
    success_threshold: int = 3    # successes to close from HALF_OPEN
    timeout: timedelta = timedelta(seconds=60)
    half_open_max_calls: int = 3  # test calls in HALF_OPEN state
```

State machine: `CLOSED → OPEN` (threshold reached) → `HALF_OPEN` (timeout elapsed) → `CLOSED` (tests pass) or `→ OPEN` (test failed).

`CircuitBreakerRegistry`: thread-safe with `asyncio.Lock`; `get_or_create(name, config)`; `reset(name)` for manual override.

## LiteLLM Router — Safe Fallback Matrix

```python
router = Router(
    model_list=[
        {"model_name": "capable", "litellm_params": {"model": "sap/anthropic--claude-sonnet-4-6"}, "priority": 1},
        {"model_name": "capable", "litellm_params": {"model": "sap/anthropic--claude-4.5-sonnet"}, "priority": 2},
        {"model_name": "capable", "litellm_params": {"model": "sap/gpt-4o"}, "priority": 3},
        {"model_name": "capable", "litellm_params": {"model": "sap/gemini-2.5-pro"}, "priority": 4},
        {"model_name": "fast", "litellm_params": {"model": "sap/anthropic--claude-haiku-4-5"}, "priority": 1},
        {"model_name": "fast", "litellm_params": {"model": "sap/gpt-4o-mini"}, "priority": 2},
        {"model_name": "fast", "litellm_params": {"model": "sap/gemini-2.5-flash"}, "priority": 3},
    ],
    fallbacks=[{"capable": ["fast"]}],  # DISABLED for write agents
    num_retries=2, timeout=30, retry_after=5, allowed_fails=3, cooldown_time=60,
)
```

**Safe fallback matrix:**
| Scenario | Safe? |
|---|---|
| Fast → fast cross-provider | ✅ Always safe |
| Capable → capable cross-provider | ✅ Safe |
| Capable → fast for WRITE agents | ❌ NEVER — quality risk |

**Write agent router**: `fallbacks=[]` — if ALL capable-tier models fail, the agent fails cleanly rather than producing incorrect financial data.

**Provider outage insight**: One Anthropic model failing on SAP AI Core likely means ALL Anthropic models fail simultaneously — must have cross-provider diversity (Anthropic + Azure + Google).

Integration: PydanticAI via `LiteLLMModel("capable")`; LangGraph via `ChatLiteLLM(model="capable")`.

Externalize config: `config/models.yaml`; `build_router(config_path, is_write_agent)` factory. Log `response.model` to detect when fallback activates.

## Layered Timeouts

```python
request = 300s    # total request timeout
llm     = 120s    # single LLM call
odata   = 30s     # single OData API call
```

Use `asyncio.timeout()` (Python 3.11+). `AdaptiveTimeoutManager`: P99 latency + 50% buffer; capped at 3× base — prevents cascade failures from slow backends.

## Bulkhead Pattern

`asyncio.Semaphore` per resource type:
- LLM calls: semaphore(5) — prevents LLM quota exhaustion
- OData calls: semaphore(20) — prevents backend overload

Separate semaphores ensure LLM quota exhaustion doesn't block OData reads (and vice versa).

## HealthChecker

`asyncio.gather` all checks concurrently:
- LLM ping: send minimal test message
- OData: fetch `$metadata` endpoint

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Error_Handling]] — exception handling and retry policies
- [[SAP_Agent_Prompt_Engineering]] — model selection cheat sheet
- [[SAP_Agent_Output_Validation]] — write-agent validation before execution
- [[Production_Reliability_MOC]] — general production reliability patterns
- [[GBrain_Architecture]] — 写操作安全设计原理同构：写代理禁止 capable→fast 降级 ≡ Fat Skills 内置验证层，越靠近写副作用操作越需要安全层

[Source: raw/SAP/resilience-deep-dive.md]
