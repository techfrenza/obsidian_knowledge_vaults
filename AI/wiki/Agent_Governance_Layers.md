---
title: "Agent Governance Layers"
parent: "[[Human_In_The_Loop]]"
aliases: ["agent-governance", "governance-first"]
tags: ["governance", "production", "security", "agent-reliability"]
category: agent-engineering
created: 2026-05-28
date: "2026-05-28"
stub: false
---

# Agent Governance Layers

Five-layer control plane defining **what an agent is allowed to do, what is audited, and how it escalates**. Governance-first mindset: build layers first, let layers earn trust, then expand authority.

> "Build the governance, then trust the agent. Not the other way around." — [@techwith_ram]

[Source: raw/Agent Governance Layers.md]

## Core Insight

Most agent failures in production are not model failures — they happen because **nobody defined the boundaries of authority**. Smarter agents make governance *more* important, not less: higher capability = higher damage potential from ambiguity.

## Layer 1: Intent Boundary

**Governs: What the agent is for.**

A separate document (not the system prompt) that every other layer references:

```markdown
# Agent Mandate

IN SCOPE:
- [specific authorized actions]

OUT OF SCOPE:
- [explicitly prohibited actions]

REQUIRES ESCALATION:
- [actions requiring human judgment]
```

**Intent creep** is the most common governance failure: the agent reasoned from its goal to a broader set of actions that served that goal, but nobody wrote down it was not supposed to do that.

## Layer 2: Permission Model

**Governs: What the agent can touch.**

Operational access control on top of conceptual intent:

- **Least privilege**: give the actual minimum permissions, not the minimum you are "comfortable" giving
- **Scoped tokens**: per-agent dedicated credentials, revocable without breaking anything else
- **Write audit**: for each write permission, ask "what is the worst case if this goes wrong?"

The permission manifest is *enforced*, not aspirational — the agent physically cannot do what is not in the manifest.

## Layer 3: Audit Trail

**Governs: What happened, when, and why.**

Should be built **second** (not last). Governance without observability is not governance — it is hope.

Structured JSONL per action:
```json
{
  "agent_id": "...",
  "session_id": "...",
  "timestamp": "...",
  "action": "...",
  "intent": "...",
  "trigger": "...",
  "result": "...",
  "escalation_triggered": false
}
```

**Write-once from agent's perspective**: agent can write, but cannot read its own audit trail. Conflating audit and memory creates an agent that can reason itself out of escalating.

## Layer 4: Escalation Protocol

**Governs: What the agent does when it does not know what to do.**

The layer separating agents that fail safe from agents that fail dangerously. Three components:

1. **Escalation triggers**: conditions from Intent Boundary's "requires escalation" section + permission blocks + uncertainty thresholds
2. **Escalation path**: pre-defined routing (security → security channel, billing → CFO, not ops)
3. **Escalation format**: structured brief with context, trigger, options, recommendation

**Critical framing**: escalation is a *success* condition, not failure. Teams that treat escalations as failures create agents that fail dangerously.

## Layer 5: Feedback Loop

**Governs: How behavior improves over time.**

Pattern: audit trail review → governance layer updates → regression testing → expanded autonomy

New agents start narrow (high escalation rate, limited permissions). As evidence builds through audits, autonomy expands based on *demonstrated* performance — not assumptions.

## Repo Layout

```
.claude/governance/
├── mandate.md          ← Layer 1: Intent Boundary
├── permissions.json    ← Layer 2: Permission Model
├── audit/              ← Layer 3: Audit Trail (write-only from agent)
│   └── YYYY-MM-DD.jsonl
├── escalation.md       ← Layer 4: Escalation Protocol
└── reviews/            ← Layer 5: Feedback Loop
    └── YYYY-MM-review.md
```

Everything version-controlled. Every change references the finding that prompted it.

## Three Failure Mode Taxonomy

| Failure | Root Layer | Symptom |
|---------|-----------|---------|
| Agent did something it should not have | Layer 1 or 2 | Intent/permission gap |
| Agent did right thing, nobody knows | Layer 3 | Missing/unreviewed audit |
| Agent made a judgment call it should not | Layer 4 | Escalation protocol not triggered |

## 关联页面

- [[Human_In_The_Loop]] — HITL is the mechanism that escalation protocol invokes
- [[Prompt_Injection]] — Attack surface that governance Layer 2-4 must address
- [[Agent_Payments_Risk_Matrix]] — Domain-specific authority boundaries for payments
- [[SAP_Agent_Guardrails]] — SAP's six-layer defense implementation
- [[Claude_Code_Security]] — Permission model for Claude Code agents
- [[Production_Agent_Engineering]] — Capability-based security as Layer 2 engineering pattern
