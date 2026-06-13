---
title: "Forward Deployed Engineering"
parent: "[[Enterprise_Agent_Playbook]]"
aliases: ["FDE", "forward-deployed-engineer", "applied-ai-deployment"]
tags: ["enterprise", "deployment", "consulting", "agent-deployment", "career"]
category: founder-playbook
created: 2026-05-28
date: "2026-05-28"
stub: false
---

# Forward Deployed Engineering (FDE)

**Definition**: The FDE is a highly skilled engineer who understands customer problems deeply, writes code into unfamiliar codebases, and communicates business impact to non-technical decision-makers. The most in-demand role in AI deployment (2026).

> "You cannot build products for an environment without actually being in the environment itself." — Palantir CTO

**Origin**: Role originated at Palantir. 2010: Special Forces used AI tools in Afghanistan; FDEs shipped code during the night based on field feedback received during the day.

[Source: raw/Forward Deployed Engineering 101.md]

## Why AI Companies Need FDEs

**Core logic chain**:
1. Intelligence is commoditizing → competitive edge is *how and where* you use it
2. No competitive advantage in intelligence alone
3. Every company needs AI, but nobody knows how to deploy it
4. An Applied AI company (with FDEs) provides access to teams that have already executed large-scale AI transformations

FDE value is a million-dollar hire because it combines three rare skills: deep customer understanding, rapid unfamiliar-codebase engineering, and executive-level business communication.

## Three-Phase FDE Job Structure

### Phase 1: Audit

**Objective**: Map processes/workflows to identify where agents create value.

Typical cadence: 2 weeks with RevOps, 1 week with Procurement, 1 month with Finance.

**Decision framework** — three questions:
1. Can the workflow be distilled into rules with variable inputs? → **Agent** (inputs: emails, PDFs, scanned images; work involves tool calls)
2. Are both rules and inputs predictable? → **Code** (faster and cheaper than an agent)
3. Does the decision require pattern recognition and domain expertise? → **Keep manual**

**Volume filter**: agents won't deliver ROI if they run 5 times per month. Target lengthy, high-volume automations.

**Prototype** at end of Audit phase. Don't overuse AI in agents — most automation requires tool calls + one orchestrating LLM call. Too much AI = unnecessary token costs that compound at scale.

### Phase 2: Evals

**Objective**: Prove the agent works to skeptical executives.

A good eval doesn't just check final answer — it verifies the agent thinks like a human would:

1. **Trace human steps**: map the human's multi-step process, grade AI on each checkpoint (not just final output)
2. **Golden examples**: sit with a human, determine what the best possible answer looks like for 3-5 tasks. That's your "great" benchmark.

Evals provide ROI evidence — not just for the engineer, but for the executive who needs to trust the deployment will deliver returns.

### Phase 3: Deployment

**Principle**: Avoid large-scale data migrations. Instead:
- Build APIs over existing data layer (SharePoint, databases)
- Place model on top as orchestrator to query through it
- Save clients from ripping out expensive ERPs they've already migrated to

**Execution environment**: create a sandbox in the company's infra to run/test/debug before hitting production.

**Production rollout**: start small, layer capabilities incrementally.
- First: agent catches bugs, investigates, writes ticket summary
- Only after that works: give it ability to write code and push PRs
- Rule: **start with smallest unit of autonomy, only then expand capability**

## 30-Day FDE Preparation Roadmap

**Week 1 (Checkpoint 1)**:
- What is an agent loop (read Anthropic Building Effective Agents)
- Two tool calls (API + web search) via Anthropic/OpenAI tool use
- Guardrails: input validation, max-step limit, output filtering
- When to use context window vs external memory
- Audit trail: log every prompt/tool call/response with timestamps

**Week 2 (Checkpoint 2)**:
- Structured outputs: always return JSON
- What breaks when taking demo to production
- Checkpoint state: save agent state every N steps for restart

**Week 3 (Checkpoint 3)**:
- Retry logic + exponential backoff (1s, 2s, 4s, 8s, cap at 16s)
- Cost optimization: cheaper models for cheap subtasks, prompt caching, cap max_tokens
- Golden dataset for evals: 20 real queries, label perfect output yourself
- Multi-agent pipelines: plan → execute → synthesize separation

**Final week**: review above, explain everything verbally tied to business metrics

## Target Backgrounds

- **Consultants/PMs**: already have ROI translation skill; gap is engineering depth — mitigate with portfolio projects
- **Software Engineers**: gap is communication — must be able to explain AI capability/limitations to non-technical VPs

## Portfolio Projects That Signal FDE Readiness

1. Production-ready AI agent executing an entire process you previously did manually (must include: API calls, autonomous logging, failure harness)
2. RAG pipeline on a domain-specific dataset (legal/medical/financial)
3. Eval framework scoring agent outputs across dimensions (correctness, format, cost, latency)
4. MCP connecting an LLM to legacy software without native AI integration

## 关联页面

- [[Enterprise_Agent_Playbook]] — Enterprise AI transformation strategy (broader context)
- [[Enterprise_Agentic_AI_6_Ideas]] — Six enterprise Agent use cases to audit for
- [[Agent_Governance_Layers]] — Governance structure FDE must build during deployment
- [[Human_In_The_Loop]] — HITL requirements FDE identifies in audit phase
- [[MCP_Enterprise_Integrations]] — Enterprise system integrations FDE deploys
- [[SAP_Agent_Overview]] — SAP-specific FDE deployment context
