---
title: SAP Agent MCP Integration
aliases: ["SAP MCP", "OData MCP server"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, MCP, OData, S4HANA, semantic-search]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent MCP Integration

MCP server acts as the abstraction layer between agents and SAP S/4HANA OData APIs. Agent contains business logic only; MCP server holds all API metadata, field mappings, and query generation.

## Architecture

```
Agent (business logic)
  → MCP Protocol
    → MCP Server (OData metadata, field mappings, query gen)
      → AppFND Destination Proxy (X-Destination-Name header)
        → S/4HANA OData API
```

Public cloud: destination `er1-s4`. Private cloud: destination `pce-001`.

## Key MCP Tool: `generate_odata_url`

Auto-detects cloud type (public/private), entity type, semantically selects fields, builds complete OData URL.

Inputs: entity type, intent (public/private), filter criteria, pagination params.
Output: complete OData URL with `$select`, `$filter`, `$top`, `$skip`, `$orderby`, `$count`.

## SemanticFieldSelector

Uses SentenceTransformer embeddings + cosine similarity to select relevant OData fields from user intent:
- Threshold: 0.3 similarity score
- Max fields: 20
- Field metadata stored in `config/public/MatchingPartner.json` with `semantic_tags` array

## OData Query Patterns (SAP-specific syntax)

```
$filter: datetime'YYYY-MM-DDT00:00:00' for dates  (NOT ISO 8601)
$filter: Amount gt 1000.00m                        (decimal suffix m)
$filter: CompanyCode eq '1010'                     (string: single quotes)
$inlinecount=allpages                              (for total count)
```

## FieldMapper — Canonical Translation

Bidirectional mapping between SAP field names and canonical names. Public vs Private Cloud differ:

| Canonical | Public Cloud | Private Cloud |
|---|---|---|
| `amount` | `HSL` | `AMOUNT` |
| `company_code` | `CompanyCode` | `BUKRS` |

```python
mapper.to_canonical({"HSL": "500"})      # → {"amount": "500"}
mapper.from_canonical({"amount": "500"}) # → {"HSL": "500"} or {"AMOUNT": "500"}
```

## DestinationServiceClient

Routes requests to S/4HANA via BTP Destination Service:
- Header: `X-Destination-Name: er1-s4` (public cloud)
- Intent extracted from A2A data part — NOT from user text
- Defaults to `"public"` if not specified

## Pagination

`fetch_all_paginated()` helper: `$top/$skip` with `$inlinecount=allpages` for total count. Auto-continues until all pages fetched.

## GuardedMCPToolset — Agent-Side Guardrails

Since MCP tools are generated from API specs and shared across agents, guardrails cannot live in the MCP server. Solution: `GuardedMCPToolset` middleware wraps `MCPServerStreamableHTTP`:

```
Agent → GuardedMCPToolset (agent-specific guardrails) → MCPServerStreamableHTTP → S/4HANA
```

`EnforceableRule` interface: `evaluate(tool_args, ctx) → RuleResult`. Rules ordered by `get_order()`. First failing rule stops the chain.

`AmountLimitRule` looks back through `ctx.messages` for prior tool responses to validate amount before allowing cancel/write operations.

See [[SAP_Agent_Guardrails_MCP]] for full implementation.

## Production Requirement: TR11

Tools MUST be exposed and consumed via MCP servers — not direct API calls. MCP Hub integration with Agent Gateway is the target architecture.

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_Guardrails_MCP]] — GuardedMCPToolset implementation
- [[SAP_Agent_Guardrails]] — agent-level guardrails
- [[MCP_Integration_Playbook]] — general MCP patterns
- [[MCP_Production_Decision_Framework]] — when to use MCP vs direct API

[Source: raw/SAP/mcp-integration-deep-dive.md, raw/MCP Server Explained.md]
