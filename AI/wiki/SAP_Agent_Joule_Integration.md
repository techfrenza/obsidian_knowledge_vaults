---
title: SAP Agent Joule Integration
aliases: ["Joule", "SAP CoPilot", "A2A Joule"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, Joule, Agent-Gateway, IAS, integration]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Joule Integration

Joule integration makes your agent a first-class participant in SAP's conversational AI surface. Three requirements: Joule knows your agent exists, both sides are authorized, both speak A2A.

## Architecture

```
Joule Chat UI
  → Agent Gateway (Agent & Tool Integration Layer)
    → Your Agent (A2A protocol)
      → Agent Gateway (callbacks, if needed)
```

Agent Gateway enforces authorization and routes requests based on UMS. Agents never receive direct calls from Joule — always via Agent Gateway.

> **Status**: Agent Gateway currently available for **internal SAP development only**. Customer-facing rollout is in roadmap.

## Step 1: Agent Registry — Make Joule Aware

Create and deploy **Joule Design-Time Artifacts** (Joule Agent Scenarios) to your Joule subscription. Define:
- **Agent Scenario**: when Joule should invoke your agent, what triggers it, exposed metadata
- **Design-Time Artifact Specification**: formal artifact deployed to Joule

Also required: ORD endpoint (TR6). Joule uses UMS catalog (fed by ORD) to recommend agents based on conversation context. Both design-time artifact AND ORD are needed.

**Future**: entitling a customer will auto-register in ATR/UMS; design-time artifact deployment step expected to go away.

## Step 2: IAS App2App Authorization — Bi-Directional

| Direction | Enables |
|---|---|
| Agent Gateway → Your Agent | Joule can send A2A requests to your agent |
| Your Agent → Agent Gateway | Your agent can send callbacks to Joule/Agent Gateway |

Setup: IAS App2App dependencies via [Cloud Identity Services: Consuming APIs](https://help.sap.com/docs/cloud-identity-services/cloud-identity-services/consume-apis-from-other-applications?version=Cloud). For AppFND agents: follow AppFND Joule Cookbook.

**Future**: Internal callers will have App2App automated via UCL Formation.

## Step 3: A2A Protocol

AppFND bootstrap agents already speak A2A correctly — no additional changes needed. Key points:
- All Agent Gateway requests arrive as `tasks/send` or `tasks/sendSubscribe`
- Respond with A2A `Task` objects including `artifacts` and `status`
- SAP extends base A2A spec with internal protocol extensions (additional context headers)

## Two Invocation Contexts

| Context | Trigger | Use Case |
|---|---|---|
| **Conversationally-triggered** | User types in Joule Chat | Most common; intent-based routing |
| **System-triggered** | Programmatic via Execution API | Automation, event-driven workflows |

Both use same auth (IAS App2App) and protocol (A2A).

## Integration Readiness Checklist

- [ ] ORD endpoint exposed and verified
- [ ] Joule Agent Scenario design-time artifact deployed
- [ ] IAS App2App: Agent Gateway → Your Agent configured
- [ ] IAS App2App: Your Agent → Agent Gateway configured
- [ ] Agent responds correctly to A2A `tasks/send`
- [ ] Tested end-to-end via Agent Gateway (not direct A2A)
- [ ] Verified in Joule Chat: discoverable, returns correct responses

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_ORD_Registration]] — TR6 (required prerequisite)
- [[SAP_Agent_UMS_Registry]] — UMS catalog (Joule discovery source)
- [[SAP_Agent_Cards]] — Agent Cards referenced in Joule Studio
- [[SAP_Agent_Ship_Checklist]] — TR3 (IAS App2App), TR7 (SPII), TR10 (Agent Gateway)
- [[MCP_Enterprise_Integrations]] — Azure AD App2App（Microsoft 生态）与 IAS App2App 是同一身份联邦问题的不同平台解法

[Source: raw/SAP/joule-integration-deep-dive.md]
