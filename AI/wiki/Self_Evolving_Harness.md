---
title: "Self-Evolving Harness"
parent: "[[Harness_Engineering_Advanced]]"
aliases: ["self-evolving-harness", "harness-self-improvement", "需要自进化的不是Agent而是Harness"]
tags: ["harness", "self-improvement", "observability", "tracing", "production"]
category: claude-tooling
created: 2026-05-28
date: "2026-05-28"
stub: false
---

# Self-Evolving Harness

**Central thesis**: The missing link between "model getting stronger" and "product getting better" is a Harness that can evolve itself. A static execution environment that doesn't learn from production data has a ceiling — the Harness needs to be the entity that improves, not just the model.

> "单独的模型已经不再是产品。模型 + harness 才是产品。"  
> "The model is the CPU, context window is RAM. The Agent Harness is the OS." — LobeHub

[Source: raw/需要自进化的不是 Agent，而是 Harness.md]

## Core Paradox

Models improve monotonically. But Harness logic written by humans becomes outdated as:
- Model interfaces change
- Tool schemas change
- User task patterns change
- Error modes change (70+ providers, each with different API/rate-limit/error formats)

If every change requires a human engineer to notice, classify, and fix — the Harness cannot keep pace with model iterations.

## Why Tracing Is the Foundational Primitive

Before a Harness can evolve itself, it needs to know what happened. **Tracing is the blackbox recorder.**

### Industry Problem: Tracing as Afterthought

Current major frameworks add observability after execution:

| Framework | Tracing Architecture | Failure Mode |
|-----------|---------------------|--------------|
| LangChain | Optional callback hooks | Forget to register = lost trace |
| CrewAI | Event bus listeners | Event loss = broken trace |
| OpenAI Agents | Explicit `trace()` creation | Not automatic, doesn't propagate |
| AG2 | Optional middleware | Not installed = zero tracing |

All follow: `execution → [manual callback/listener/middleware] → trace`

### LobeHub Solution: Step-Native Tracing

Architecture: `single-step execution → trace event is natural side effect`

Each `run step` is an event boundary. Tracing is the execution's *byproduct*, not a post-install feature.

**Agent Operation Tracing (Execution Snapshot)**:
```
Agent Operation  op_123456  claude-sonnet-4-6  6 steps  45.2s
├─ Step 0  [call_llm]  3.1s
│  ├─ LLM     in:4.2k out:156 tokens  cache:87%
│  └─ Output  I need to search...
├─ Step 1  [call_tool]  2.4s  search_documents ✓
├─ Step 2  [call_llm]  8.7s
│  ├─ LLM     in:8.1k out:342 tokens  cache:92%
│  ├─ → 2 tool_calls: [edit_file, read_file]
...
└─ done  tokens=8.4k  cost=$0.0423  cache:84%  hit:7.2k  miss:1.2k
```

Captures: model used, token costs per step, step type, latency, cache rate, error context + **Context Engine state at error time**.

## Error Pattern 自动巡检 — Production Case Study

LobeHub's first self-evolution capability, deployed April 2026.

### Problem
70+ providers each with distinct error formats. Error handling is *reactive* — you cannot design for error classes you don't know exist. Human classification speed cannot match error generation speed at scale.

### Solution: Agent-Driven Error Pattern Inspection

7-step automated loop:
1. **Data collection**: Pull recent error records, bucket by provider/errorType/statusCode/message
2. **Pattern recognition**: Compare against existing ERROR_PATTERNS, identify uncovered patterns
3. **Auto-classification**: Separate user-side errors (quota/rate-limit) from Harness bugs (schema incompatibility, context overflow)
4. **Auto-fix**: For user-side errors, update matching rules directly
5. **Auto-commit**: commit → push → open PR
6. **Auto-cleanup**: Delete historical noise matching new patterns
7. **Root cause analysis**: For Harness bugs, deep analysis + create fix Task requiring human confirmation

### Results (9 runs):
- Cumulative patterns: 31 → 104, stabilized (new patterns converged to 0)
- 20+ Harness bugs discovered autonomously (Tool schema incompatibility, negative `max_tokens`, DeepSeek `reasoning_content` loss, Context Window overload)
- Agent success rate: 75% → 95%

## Four Levels of Harness Self-Evolution

```
L1: Pure manual    Human discovers → Human analyzes → Human fixes
L2: Agent assists  Agent flags → Human confirms → Agent executes partial fix
L3: Agent leads    Agent: collect/identify/modify/PR/validate  (human: boundary judgment)
L4: Autonomous     Agent: optimize Context Engine strategy, adjust Tool schemas, predict errors
                   Human: set goals only
```

LobeHub Error Pattern system is at **L3**. L4 is the roadmap target.

## Why Consumer Products Are Self-Evolving Harnesses

**Traditional SaaS**: Product = feature code + user data. Features are fixed. Improvement requires human-written version updates.

**AI-Native Product**: Product = Harness (runtime) + interaction data + evolution capability.

Every user interaction provides evolution signal — not to train the model (the model is external/generic), but to optimize the runtime.

**Competitive moat equation**:
- Model: universal, external, replaceable
- Harness: proprietary, internal, continuously accumulating

**Self-hosted products (Hermes/OpenClaw) face signal density ceiling**: 10s of agent executions per user per day → sparse feedback → slow iteration. A consumer-aimed SaaS product may have 10k+ executions per day → patterns discovered in minutes, deployed same day.

## The Bitter Lesson Applied to Harnesses

Rich Sutton's lesson: "general methods + compute beats handcrafted domain knowledge."

In the agent era: **hand-written Harness logic becomes outdated as models improve**. LangChain rebuilt Open Deep Research 3x in one year. Manus rebuilt Harness 5x in 6 months.

The only solution: **let the Harness evolve itself**, rather than waiting for humans to refactor.

## Three Planned Evolution Dimensions

1. **Agent-level auto-evolution**: Each agent nightly replays daily Operation Tracings, analyzes failure patterns, self-adjusts Prompt + execution strategy. Delivers "Brief" to user next morning.
2. **User-level auto-evolution**: Nightly analysis of interaction patterns + auto-update of Persona memory.
3. **Global Eval Harness**: Every Failed Task → evaluation → attribution → fix loop. Same failure never repeats.

**Constraint**: All evolution requires human authority boundaries. Agent handles high-frequency, verifiable, low-risk work. Humans retain goal-setting, risk judgment, final decisions.

## 关联页面

- [[Harness_Engineering_Advanced]] — Static Harness engineering patterns (basis for evolution)
- [[Harness_Over_Model_Principle]] — The core thesis: Harness determines product quality, not model
- [[Agentic_Memory_System]] — Memory architecture that feeds Harness evolution data
- [[Claude_Code_Self_Evolving]] — Claude Code's self-evolving mechanisms
- [[Institutional_Evolution_Flywheel]] — Organizational analog of Self-Evolving Harness
- [[Production_Reliability_MOC]] — Production reliability patterns including tracing
