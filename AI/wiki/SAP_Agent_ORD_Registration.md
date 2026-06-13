---
title: SAP Agent ORD Registration
aliases: ["ORD", "UMS", "UCL", "agent catalog"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, ORD, UMS, discovery, production-requirement]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent ORD Registration

Open Resource Discovery (ORD) is the SAP standard for exposing machine-readable agent metadata. Required for production (TR6). Enables Joule recommendations, Agent Gateway routing, and LeanIX governance.

## What It Does

UCL polls your ORD endpoint → writes to UMS → enables:
- **Joule** to recommend your agent based on conversation context
- **Agent Gateway** to route A2A requests to correct agent instance
- **LeanIX AI Agent Hub** to govern agent landscape

## Three Endpoints Required

| Endpoint | Purpose | Auth |
|---|---|---|
| `GET /.well-known/open-resource-discovery` | ORD config — lists both documents | **None** (open) |
| `GET /open-resource-discovery/v1/documents/system-version` | Static doc — global, landscape-agnostic | **None** |
| `GET /open-resource-discovery/v1/documents/system-instance` | Dynamic doc — tenant-specific, accepts `?local-tenant-id` | **None** |

All ORD endpoints are open (no auth) — required by ORD spec for UCL crawling. Business A2A endpoints remain JWT-protected.

## Implementation: Use the Skill

**Do not hand-roll ORD files.** Use the `sap-agent-ord-endpoint` skill:
```
/sap-agent-ord-endpoint
```

The skill runs 5 phases:
1. **Read config**: extracts `AGENT_ORD_ID` from `app.yaml`, `AGENT_TITLE` + `AGENT_DESCRIPTION` from `app/main.py` (AgentCard)
2. **Ask for CPA namespace**: one thing the skill cannot infer — your product's ORD namespace (e.g., `sap.fin`). Get it from your PM or UCL onboarding request. **Never guess** — wrong namespace causes ORD ID collisions.
3. **Declare dependencies**: `ord-dependencies.json` template → fill in AI Core, Object Store, Destination Service, etc.
4. **Create ORD files**: `app/ord.py` (Starlette handlers) + `app/ord/document-system-version.json` + `app/ord/document-system-instance.json`
5. **Test locally**: `curl` all 3 endpoints, verify required fields

## ORD ID Naming

```
{namespace}:agent:{agent-id}:v1
```
Example: `sap.fin:agent:apar-clearing-agent:v1`

`{agent-id}` = `metadata.name` from `app.yaml`. `{namespace}` = CPA namespace from onboarding request.

## Runtime URL Injection

`{{AGENT_BASE_URL}}` is NOT baked in at build time. Set `AGENT_PUBLIC_URL` environment variable in deployment config — ORD documents always contain the correct deployed URL.

## app.yaml Auth Update

ORD paths must be `no_auth`:
```yaml
service:
  apiAuth:
    - path: /v1/data/{**}
      type: jwt      # protect business endpoints
    - path: /*
      type: no_auth  # ORD paths open
```

## Production Checklist

- [ ] ORD endpoint returns valid JSON at `/.well-known/open-resource-discovery`
- [ ] Both ORD documents reachable without auth
- [ ] `AGENT_NAMESPACE` confirmed with PM (not guessed)
- [ ] `AGENT_PUBLIC_URL` set in deployment config
- [ ] Agent Card URL in `resourceDefinitions` resolves
- [ ] Verified in UMS catalog after deployment

## ORD vs Agent Card

| | ORD Document | Agent Card |
|---|---|---|
| Purpose | UMS catalog metadata | A2A client discovery |
| Endpoint | `/.well-known/open-resource-discovery` | `/.well-known/agent.json` |
| Consumer | UMS, UCL, Agent Gateway | A2A callers, Joule Studio |

Both required. ORD `resourceDefinitions` references Agent Card URL.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Cards]] — Agent Card (complements ORD)
- [[SAP_Agent_UMS_Registry]] — UMS consumes ORD
- [[SAP_Agent_Joule_Integration]] — Joule uses UMS populated by ORD
- [[SAP_Agent_Ship_Checklist]] — TR6 context
- [[Claude_Code_Skills]] — 发现范式对比：SAP ORD + UMS（中心化强制目录）≡ agentskills.io SKILL.md（去中心化标准）— 两种"AI能力发现与路由"解法

[Source: raw/SAP/ord-registration-deep-dive.md]
