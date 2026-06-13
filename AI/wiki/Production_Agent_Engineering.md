---
title: "Production Agent Engineering Stack"
parent: "[[Agent_Harness_Engineering]]"
aliases: ["missing-engineering-stack", "production-agent-stack", "agent-production-stack"]
tags: ["production", "tokens", "security", "trust", "skills", "agent-engineering"]
category: agent-engineering
created: 2026-05-28
date: "2026-05-28"
stub: false
---

# Production Agent Engineering Stack

Four primitives that determine whether an agent survives contact with real users, real data, and real adversaries. The gap between "demo" and "production" is these four surfaces.

> "A production agent is not a model and a prompt. It's a token economy, a skill catalog with versioning, a capability-scoped security model, and a trust telemetry stack." — @karlmehta

[Source: raw/The Missing Engineering Stack for Production AI Agents.md]

## Primitive 1: Token Economy (Context Discipline)

**Treat tokens like 1990s embedded memory: budget every byte, evict aggressively.**

### Prompt Caching
- Anthropic `cache_control: { type: 'ephemeral' }` (5-min TTL default, 1-hour via extended-TTL beta)
- Cached tokens: **10% of input cost**; cache writes cost 25% more on first call
- **Ordering matters**: cache is a prefix store, not content-addressable. Byte-identical span must be at start.
- Two cache breakpoints (tool catalog + skills bundle): either can evolve without busting the other

### Model Routing (3-Tier)
| Tier | Use Case | Model | Cost Context |
|------|----------|-------|-------------|
| Retrieval/classification/extraction | Structured outputs, forced JSON | Haiku | $1/$5 per M tokens |
| Synthesis/reasoning over context | 80% of business logic | Sonnet | $3/$15 per M tokens |
| Planning/tool selection/disambiguation | >5 tool calls, ambiguous intent | Opus | $15/$75 per M tokens |

**Route by step type, not input length.** 4–8× cost amortization on production workloads typical.

### Structured Output Dodge
Force tool_choice to receive typed JSON instead of freeform. Skip 50–80% of freeform tokens. Pair with strict mode (OpenAI) or JSON Schema with `$defs` (Anthropic).

## Primitive 2: Skill Composition

**Separate identity/capabilities/policies into composable fragments assembled at runtime.**

### Trigger/Action/Restriction Triple Per Skill
```json
{
  "id": "refund-policy-2024",
  "trigger": "the user asks for a refund",
  "action": "verify order within 30-day window, issue via tools.stripe.refund, confirm via email",
  "restriction": "never issue refunds > $500 without human approval; no refunds in first cycle"
}
```

Domain experts (PMs, ops, legal) author triples in plain English. **Versioning per skill, not per agent.** Eval suites attach to the skill — swapping a policy doesn't require re-blessing the entire agent.

### Tool Design Principles
- **Strict JSON schemas** with `additionalProperties: false` — closed-world schemas catch hallucinated arguments at validator, not in production
- **Small and idempotent**: `orders.refund(orderId, amountCents)` not `orders.handle(intent, payload)`

### MCP Transport Selection
| Transport | Use Case | Tradeoff |
|-----------|---------|---------|
| stdio | Code execution, filesystem, sensitive ops | Lowest latency, zero network surface |
| SSE | Browser-friendly hosted tools | ~50ms latency |
| StreamableHTTP | Current recommendation for hosted MCP | Compatible with cloud LB |

### Plan-Execute-Review Loop
For agents with >3 sequential tool calls:
1. **Plan** (1 message, no tool calls)
2. **Execute** (n messages, tool calls only)
3. **Review** against plan's stated success criteria (1 message, no tool calls)

Anthropic Agent SDK exposes this via `plan_mode` primitive.

## Primitive 3: Capability-Based Security

**Object capability model: hand the smallest unforgeable token that lets the agent do exactly what it needs.**

### Threat Surface
- Prompt injection (adversarial input in retrieved context/tool outputs)
- Data exfiltration (agent calls tool emitting sensitive data to attacker-controlled destination)
- Tool abuse/RCE (legitimate tool used unexpectedly)
- Supply chain (tool dependency or model weight compromised)
- Secret leakage (API keys in logs, prompts, or error messages)

### Authorization Pattern
- Per-session tokens, scoped to specific endpoints, with TTL
- OAuth 2.1 with PKCE for delegated authorization
- Tokens stored in OS keychain (libsecret/Keychain/DPAPI)
- **Never give long-lived admin keys to agents**

### Tool Sandboxing (Ranked by Overhead)
1. **WASM** (Wasmtime/Wasmer): sub-ms startup, deny-by-default I/O → best for code execution + policy tools
2. **gVisor**: userspace kernel, near-full Linux compat, 10–100ms startup → tool subprocesses needing POSIX
3. **Firecracker**: microVM, ~125ms startup, hardware isolation → multi-tenant shared infra

### Prompt Injection Defenses That Actually Work
- **Channel separation**: wrap untrusted content in labeled XML tags, tell model to ignore instructions inside
- **Allowlist tool surfaces**: `send_email` only to per-conversation allowlist
- **Output content classifiers**: small model scans tool calls before execution (suspicious destinations, base64 blobs)
- **HITL gates**: anything costing money/sending external comms/modifying DB/touching PII requires approval

### Cisco DefenseClaw (OSS, Apache 2.0)
Announced RSAC 2026. Four components:
- **Skills Scanner**: capability scan before execution
- **MCP Scanner**: allow/block on MCP server inspection
- **CodeGuard**: static analysis for secrets/unsafe deserialization/weak crypto/injection
- **Guardrail Proxy**: runtime inspection of prompts, completions, tool calls (regex + optional LLM judgment)

## Primitive 4: Trust Telemetry

**"It worked when I tested it" is not a trust story.**

### Four Signals Required in Production

1. **Eval pass rate**: regression suite against golden set; tag failures by skill; run on every prompt/model/tool change
2. **Drift detection**: track distribution shift on input embeddings (cosine distance from reference centroid); alarm at 2σ, investigate at 1σ
3. **Behavioral canaries**: N synthetic inputs/day targeting injection/exfil/jailbreak surfaces; when new attack class appears, add to canary set
4. **Audit trail with integrity**: JSONL of all runs; hash chain over events; anchor head to immutable store (S3 Object Lock, GCS Bucket Lock)

### TrustScore
Composite: `weighted(eval_pass_rate, drift_score, canary_survival, HITL_approval_rate)`. Operationally meaningful only if grounded in underlying signals.

### Compliance Integrations
- **TrustModel.ai**: GRC overlay (NIST AI RMF, ISO 42001, EU AI Act, SOC 2, FedRAMP)
- **OpenTelemetry GenAI**: standard spans (`gen_ai.system`, `gen_ai.request.model`, `gen_ai.usage.input_tokens`)

## 关联页面

- [[Agent_Governance_Layers]] — The authorization/audit/escalation layer wrapping this stack
- [[Prompt_Injection]] — Security primitive this stack defends against
- [[Claude_Code_Security]] — Permission model specifics
- [[Agentic_Memory_System]] — Memory architecture that feeds agent context
- [[Skill_Engineering_10_Rules]] — Skill design engineering complement to this stack
- [[SAP_Agent_Guardrails]] — Enterprise guardrails implementation
- [[Production_Reliability_MOC]] — Reliability patterns
