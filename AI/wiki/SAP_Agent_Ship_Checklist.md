---
title: SAP Agent Ship Checklist (TR1–TR14)
aliases: ["SAP TR checklist", "agent production readiness"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, production, ship-checklist, metering, TR-requirements]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Ship Checklist (TR1–TR14)

14 Technical Requirements for shipping a production SAP agent. Status below may be out of date — always verify at the authoritative [AI Golden Path Ship Guide](https://pages.github.tools.sap/agent-release-paved-path/ai-golden-path/build/agents/ship/).

## Technical Requirements

| # | Requirement | Status |
|---|---|---|
| TR1 | Agent exposed as A2A Server | ✅ Available |
| TR2 | Agent runs on AppFND (or exception granted) | ✅ Available |
| TR3 | Agent accepts IAS App2App tokens | ✅ Available |
| TR4 | Agent onboarded to Unified Services | ⚡ Prepare Today |
| TR5 | Agent onboarded to UCL | ⚡ Auto-via-TR4 on AppFND |
| TR6 | Agent exposes ORD endpoint | ⚡ Use `sap-agent-ord-endpoint` skill |
| TR7 | Agent implements SPII for Agent Gateway | ⚡ Prepare Today |
| TR8 | BTP Test Blueprint available | ⚡ Prepare Today |
| TR9 | E2E test for DWC defined | 🔲 To be Clarified |
| TR10 | Agent uses Agent Gateway for outbound calls | ✅ Available |
| TR11 | Agent uses MCP servers for tool calls | ⚡ Prepare Today |
| TR12 | Agent instrumented with OpenTelemetry | ✅ `auto_instrument()` |
| TR13 | Agent emits metering payloads via OTel | ⚡ Prepare Today |
| TR14 | Agent supports extensibility | ⚡ Use `sap-agent-extensibility` skill |

## Agent Step Metering (TR13)

Commercial billing unit: **one agentic loop iteration = one Agent Step**.

**Counts as 1+ steps:**
- LLM call + OData read/write
- LLM call + database query
- LLM call for planning or formatting
- PDF/OCR processing (1–n steps)

**Counts as 0 steps:**
- Deterministic API call with no LLM
- Retry after technical failure
- Technical error
- Memory Service lookups
- Logging and audit calls

**Agent Tiers** (board moving toward single tier — verify current status):
| Tier | AI Units/Step | Profile |
|---|---|---|
| Basic | 5 | Low complexity, simple tool use |
| Standard | 10 | Moderate complexity, multiple tools |
| Advanced | 25 | High complexity, sophisticated reasoning |

Tier set jointly by LoB / Controlling / BAI / BMP — not engineering team unilaterally.

**Steps to implement TR13**:
1. Understand Agent Step concept (one loop iteration = one step)
2. Request AI Feature ID from pricing team via Jarvis AI Onboarding
3. Identify where steps should be emitted in your code
4. Implement using metering code snippets from metering knowledge base

## PM Gates (Summary)

| Gate | Tool | What |
|---|---|---|
| PM1 | AHA → Jarvis | Create Jarvis record, set GA/RTC date |
| PM2 | Jarvis | Value Assessment |
| PM3 | Jarvis AI Onboarding | Tier, Feature ID, Floor Pricing |
| PM5 | Jarvis | Implement metering (links to TR13) |
| PM8 | QMS/Sirius | E2E validation and release decision |

Finance Autonomous Domain agents: use Jira template [FINSPENDCODEBASEDAGENTSINIT-1](https://jira.tools.sap/browse/FINSPENDCODEBASEDAGENTSINIT-1).

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_ORD_Registration]] — TR6 detail
- [[SAP_Agent_Joule_Integration]] — TR3, TR7, TR10 detail
- [[SAP_Agent_MCP_Integration]] — TR11 detail
- [[SAP_Agent_Testing]] — TR8, TR9 context
- [[SAP_Agent_UMS_Registry]] — TR4, TR5 detail
- [[Tokenmaxxing]] — 计量粒度对比：Agent Step（业务价值单元）vs Token（算力消耗）— 企业 AI 定价范式迁移的早期信号

[Source: raw/SAP/ship-checklist-deep-dive.md]
