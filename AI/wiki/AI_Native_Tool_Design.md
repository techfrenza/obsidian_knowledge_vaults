---
title: "AI-Native Tool Design"
parent: "[[MCP_Production_Decision_Framework]]"
aliases: ["agent-first-tools", "ai-native-api", "tool-design-for-agents"]
tags: ["mcp", "tool-design", "ai-native", "context-engineering"]
category: agent-engineering
created: 2026-05-28
date: "2026-05-28"
stub: false
---

# AI-Native Tool Design

**Core thesis**: Most "AI-first" products are thin wrappers — they expose APIs to AI without redesigning for AI's fundamental constraints. True AI-native tool design requires internalizing that AI has no memory, doesn't browse (only executes), and needs precision where humans need abstraction.

> "传统 API 设计面向人类开发者，核心是以保护为目的的抽象。AI 原生设计正好反过来：agent 不会被复杂的报错吓跑，但它会被模糊的报错困住。"

[Source: raw/从 Zero 到 Cloudflare：为 AI 重写工具，不只是把 API 包一层.md]

## Three Design Constraints Unique to AI Consumers

### 1. AI Has No Memory (Every Session Starts at Zero)

Human engineers accumulate institutional knowledge over years. Agents restart from zero on every session. The onboarding knowledge humans gain implicitly must be *explicitly co-delivered with the tool*.

**Solutions**:
- **Vercel Zero**: `zero skills get zero --full` — agent reads Markdown operational guide *from the compiler itself*, version-locked to match what's installed
- **AGENTS.md**: inject project background, build commands, code conventions into every session's context
- **Instruction budget**: LLMs reliably follow 150–200 instructions maximum. Every extra rule competes for attention. Unlike humans, agents treat every line as an instruction to execute.

**Salesforce Headless 360** example: business context (open escalations, 30-day renewal windows, SLA violations) was previously only accessible via UI. AI-native version encodes it as directly consumable data in agent-accessible APIs.

### 2. AI Doesn't Browse — It Executes

Humans scan a 500-item menu and find what they need. Agents can't. With 100 tools, accuracy at selection degrades dramatically. **Anthropic data**: 134K token tool definitions → Opus 4 accuracy drops to 49%.

**Anthropic recommendation**: keep core toolset at ~12 tools.

**Solutions**:
- **Cloudflare Code Mode MCP**: 2,594 endpoints → 2 tools (`search` + `execute`). Agent writes JavaScript to call APIs, runs in isolated sandbox. Token count: 1M+ → ~1,000. Rationale: code generation accuracy >> tool selection accuracy.
- **Stripe Agent Toolkit**: hand-pick 12-15 most critical operations from hundreds of endpoints. Changed assumption: from "readable by human developers" to "discoverable at runtime by AI systems."

**Decision**: minimize tool count. When tool count is unavoidable, give the agent a way to *write code* that calls the underlying APIs.

### 3. AI Needs Precision, Humans Need Abstraction

Traditional API: `APIFailureError: operation failed, try again` — friendly to humans (hides TCP/DNS complexity), fatal to agents (agent cannot identify what to fix, breaks the try-feedback-repair loop).

AI-native API: expose raw `ConnectTimeoutError` with full stack trace and context. Information density that overwhelms humans is *exactly right* for agents.

**Vercel Zero example**: `NAM003` → `declare-missing-symbol` — stable, machine-matchable repair ID. Natural language error messages have version drift and parsing ambiguity; stable codes break the cycle deterministically.

## What "Thin Wrapper" Looks Like (Anti-Pattern)

Most MCP servers: 1:1 mapping of API endpoints → MCP tools.

- Format is correct
- Guidance knowledge is missing ("when to use, in what order, how to recover from errors")
- Leverage tools are missing (tools that abstract away agent-error-prone tasks into deterministic operations)

The "引导知识" (guidance knowledge) and "杠杆工具" (leverage tools) need to be co-delivered with the core API.

## Design Checklist for AI-Native Tools

| Constraint | Diagnosis | Fix |
|-----------|-----------|-----|
| No memory | Does agent have context beyond API docs? | Co-deliver AGENTS.md/skill guide with tool |
| Can't browse | >12 tools in catalog? | Reduce surface area or provide code-execution escape hatch |
| Needs precision | Error messages in natural language? | Add stable error codes + repair IDs |
| No memory | Tool descriptions reference institutional knowledge? | Make implicit knowledge explicit in descriptions |
| Can't browse | Tool descriptions too similar to each other? | Redesign as `search + execute` pattern |

## Progress by Layer (2026)

| Layer | Status |
|-------|--------|
| Platform (Salesforce, Stripe, Atlassian, AWS) | Agent-first on roadmap core |
| Protocol (MCP standardization) | Consolidating |
| Security | Early stage |
| Compiler/language layer (Vercel Zero) | Experimental |

## 关联页面

- [[MCP_Production_Decision_Framework]] — When and how to deploy MCP tools
- [[MCP_Connectors]] — MCP ecosystem overview
- [[Context_Engineering]] — Context as the engineering surface agents consume
- [[Skill_Engineering_10_Rules]] — Skill design principles (tool from agent perspective)
- [[Tokenmaxxing]] — Token optimization that AI-native tool design enables
- [[Prompt_Injection]] — Security threat in AI-native tool consumption
- [[Generative_UI_Architecture]] — Agent 运行时动态生成 UI 的三种模式（AI-Native 工具设计的前端实现层）
