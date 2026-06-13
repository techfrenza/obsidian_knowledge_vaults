---
title: SAP Agent Cards
aliases: ["Agent Cards", "agent.json", "A2A discovery"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, agent-cards, A2A, discovery, registry]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Cards

Agent Cards are JSON documents served at `/.well-known/agent.json` describing an agent's capabilities for A2A discovery. Required for production. Distinct from ORD documents (which serve UMS/catalog).

## Naming Convention

`pc-{domain}-{function}-agent`

Examples: `pc-fin-journal-entry-agent`, `pc-fin-smartclearing-agent`, `pc-q2c-billing-adjustment-agent`

## Agent Card JSON Schema

| Field | Type | Notes |
|---|---|---|
| `name` | string | Pattern: `^[a-z0-9-]+$` |
| `displayName` | string | Human-readable |
| `description` | string | maxLength: 500 |
| `version` | string | Semver (1.2.0) |
| `protocol` | string | `a2a/1.0` or `a2a/2.0` |
| `status` | string | `development` / `beta` / `production` / `deprecated` |
| `capabilities` | object | `streaming`, `multiTurn`, `fileUpload` booleans |
| `intents` | array | Each with `examples[]` (≥3), `requiredEntities[]` |
| `metadata` | object | `domain`, `owner`, `contact` — all required |
| `dependencies` | array | OData services, MCP servers |
| `constraints` | object | Rate limits, data sensitivity, max batch size |

## Finance Domain Agent Cards

**`pc-fin-journal-entry-agent`** v1.2.0 production
- Intents: `CREATE_JOURNAL_ENTRY`, `VALIDATE_DATA`, `SPLIT_DATA`, `GET_POSTING_STATUS`
- OData: `API_JOURNALENTRYITEMBASIC_SRV`

**`pc-fin-smartclearing-agent`** v1.0.0 production
- Intents: `FIND_CLEARING_PROPOSALS`, `EXECUTE_CLEARING`, `ANALYZE_OPEN_ITEMS`
- OData: `API_OPLACDOC_PROCESS_SRV`

**`pc-fin-acc-accruals-agent`** v1.0.0 beta
- Intents: `CREATE_ACCRUAL_PROPOSAL`, `ANALYZE_HISTORICAL_DATA`, `EXTRACT_POLICY_INFO`

## AgentRegistry (Python class)

Central registry indexed by domain tag:
```python
registry.register(url)                      # fetches /.well-known/agent.json
registry.find_by_domain("finance")          # all finance agents
registry.find_by_intent("EXECUTE_CLEARING") # agent handling this intent
registry.search("clearing")                 # keyword search
registry.health_check_all()                 # ping all registered agents
```

## AgentCardValidator

JSON Schema validation + custom checks:
- ≥3 examples per intent (required)
- Required metadata fields: `domain`, `owner`, `contact`
- Valid semver version string
- Valid status enum value

## Deprecation Pattern

```json
{
  "compatibility": {
    "deprecatedIntents": [{
      "intent": "LEGACY_CLEAR",
      "replacedBy": "EXECUTE_CLEARING",
      "removeInVersion": "2.0.0"
    }]
  }
}
```

## ORD vs Agent Card

| | ORD Document | Agent Card |
|---|---|---|
| Purpose | Metadata for UMS catalog/registry | A2A capability description |
| Endpoint | `/.well-known/open-resource-discovery` | `/.well-known/agent.json` |
| Consumer | UMS, UCL, Agent Gateway, Joule | A2A callers, Joule Studio |
| Required | Yes (TR6) | Yes (TR1) |

Both required for production. ORD document references the Agent Card URL in its `resourceDefinitions`.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Multi_Agent]] — A2AClient uses agent cards for discovery
- [[SAP_Agent_ORD_Registration]] — ORD endpoint (complements agent card)
- [[SAP_Agent_UMS_Registry]] — UMS consumes ORD which references agent card
- [[SAP_Agent_Joule_Integration]] — Joule Studio reads agent cards

[Source: raw/SAP/agent-cards-deep-dive.md]
