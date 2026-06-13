---
title: SAP Agent UMS Registry
aliases: ["UMS", "UCL registry", "agent discovery"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, UMS, registry, discovery, Joule]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent UMS Registry

Unified Metadata Service (UMS) is SAP's central platform for agent and tool metadata across all SAP landscapes. Built on the ORD standard — exposing an ORD endpoint is the only action required.

## Two Perspectives

| Perspective | Also Called | Contains | Consumers |
|---|---|---|---|
| **system-version** | Catalog / Static | Agents that *could* be deployed. Global. | Joule recommendations, Discovery Center, LeanIX AI Agent Hub |
| **system-instance** | Registry / Dynamic | Agents currently *deployed* in specific tenant. | Agent Gateway routing, Joule Studio, HITL metadata |

Your ORD endpoint exposes both. UCL polls and writes both to UMS automatically (on AppFND).

## Data Flow

```
Your Agent (ORD endpoint)
  → UCL (polls automatically on AppFND)
    → UMS
      ├── Static (system-version catalog) → Joule, Discovery Center, LeanIX
      └── Dynamic (system-instance registry) → Agent Gateway, Joule Studio
```

## What Consumers Get

**Agent Gateway**: queries dynamic perspective → verifies agent deployed and accessible for tenant → routes A2A request to correct `baseUrl`.

**Joule**:
- Static → recommends agents based on conversation context (even if not yet in user's tenant)
- Dynamic → verifies agent is actually deployed before routing

**LeanIX AI Agent Hub**: governance and discovery of SAP-developed and third-party agents across landscape.

**Joule Studio Agent Builder**: shows available agents for multi-agent composition.

## Required ORD Fields for UMS

| ORD Field | UMS Use |
|---|---|
| `agents[].ordId` | Unique ID: `{namespace}:agent:{id}:v1` |
| `agents[].title` | Display name in catalog UIs |
| `agents[].description` | Joule matching and recommendations |
| `apiResources[].resourceDefinitions[].url` | Agent Card URL for A2A discovery |
| `describedSystemInstance.baseUrl` | Runtime URL for Agent Gateway routing |
| `integrationDependencies` | Services the agent depends on |

All populated by `sap-agent-ord-endpoint` skill templates.

## For AppFND Agents: No Manual Steps

UCL registration and UMS sync are automatic. The only required action is implementing the ORD endpoint (TR6) via the `sap-agent-ord-endpoint` skill.

## Verifying Registration

After deploy, use the [UMS Discovery API](https://pages.github.tools.sap/ums/documentation/docs/use-cases/agent-catalog-and-registry/):
- Catalog query (static): all SAP-developed agents that could be deployed
- Registry query (dynamic): agents deployed in specific landscape
- Metadata retrieval: endpoint URLs, capabilities, Agent Card details

## Static vs Dynamic in Practice

**Static** populated when ORD `system-version` document crawled — represents product/release level. Used by Joule for suggestions regardless of which tenant is active.

**Dynamic** populated when ORD `system-instance` document crawled with `?local-tenant-id`. `describedSystemInstance.localId` populated at request time from the query parameter.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_ORD_Registration]] — ORD endpoint implementation
- [[SAP_Agent_Cards]] — Agent Card (referenced by ORD)
- [[SAP_Agent_Joule_Integration]] — how Joule uses UMS
- [[SAP_Agent_Ship_Checklist]] — TR4 (Unified Services) and TR5 (UCL) context

[Source: raw/SAP/agent-registry-ums-deep-dive.md]
