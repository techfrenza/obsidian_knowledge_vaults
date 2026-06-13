---
title: SAP Agent Guardrails
aliases: ["GuardedMCPToolset", "SAP safety gates"]
parent: "[[SAP_Agent_Overview]]"
tags: [SAP, guardrails, security, enterprise-AI, validation]
category: sap-agents
created: 2026-05-20
date: "2026-05-20"
---

# SAP Agent Guardrails

6-layer defense-in-depth for enterprise SAP agents. Critical architectural constraint: **Layers 3+4 (Prompt + LLM) are NOT independent** — both rely on LLM following instructions. Adversarial prompts bypass both. For write agents, NEVER rely solely on soft guardrails.

## 6-Layer Architecture

| Layer | Type | What It Checks |
|---|---|---|
| 1: Input Validation | HARD | Length limits, format validation, file type checks |
| 2: Intent Guardrails | HARD | Blocked intents: `DELETE`, `ADMIN_ACCESS` |
| 3: Prompt Guardrails | SOFT | XML rule injection into system prompt |
| 4: LLM Processing | SOFT | Model follows rules (unreliable against adversarial input) |
| 5: Output Validation | HARD | Credential redaction, PII scrubbing |
| 6: Action Guardrails | HARD | Amount limits, batch size caps |

## YAML Configuration

**Base config** (`guardrails/config.yaml`):
```yaml
limits:
  max_batch_size: 100
  max_amount: 1000000
  confirm_threshold: 10000  # require HITL above this amount
  max_llm_calls: 10
  max_odata_calls: 50
  max_processing_time_seconds: 300
```

**Domain config** (`guardrails/finance.yaml`): extends base; `allowed_company_codes`, `allowed_ledgers`, `fiscal_year_check`, currency-specific thresholds (EUR confirm 10000 / block 1000000).

**Role config** (`guardrails/roles/accountant.yaml`): `can_post: false`, `max_amount: 50000`.

## Key Classes

- `MultiLayerGuardrails`: `validate_input()`, `check_intent()`, `get_prompt_guardrails()`, `validate_output()`, `check_action()`
- `GuardrailChain`: sequential; stops on first `block`; collects warnings
- `AsyncGuardrails`: `asyncio.gather(return_exceptions=True)` for parallel validation
- `ContextualGuardrails`: lower thresholds at month-end; reduce limits after 3 failed attempts
- `GuardrailAuditLogger`: full audit trail for SOD (Segregation of Duties) compliance

## Custom Rules

```python
AmountLimitRule(max_amount)   # blocks if action amount > limit
TimeWindowRule(allowed_hours) # restricts to business hours
CompanyCodeRule(allowed_codes) # restricts to authorized company codes
CompositeRule([rule1, rule2], logic="AND")  # AND/OR composition
```

`DynamicRuleLoader`: loads rules from config dict; extensible `RULE_TYPES` registry.

## GuardedMCPToolset — MCP-Level Guardrails

Guardrails for MCP tools must live on the agent side (not in the MCP server) because:
1. MCP tools are generated from API specs — cannot embed business rules
2. Same MCP server serves multiple agents with different guardrail needs

Pattern: `GuardedMCPToolset` wraps `MCPServerStreamableHTTP`, pre-validates each tool call:
```python
guarded_toolset = GuardedMCPToolset(
    inner_toolset=mcp_server,
    guardrail_configuration=ToolsetGuardrailConfiguration([bd_cancellation_guardrails])
)
agent = Agent(model=..., toolsets=[guarded_toolset])
```

`EnforceableRule.evaluate(tool_args, ctx) → RuleResult`. `AmountLimitRule` reads prior tool responses from `ctx.messages` to validate amounts before write operations.

## Finance Error Templates

Predefined error messages for common violations:
- `FISCAL_PERIOD_CLOSED` — posting to closed period
- `DEBIT_CREDIT_IMBALANCE` — journal entry doesn't balance
- `GL_ACCOUNT_NOT_FOUND` — account doesn't exist in company code
- `UNAUTHORIZED` — role limit exceeded

## Related

- [[SAP_Agent_Overview]] — full stack
- [[SAP_Agent_MCP_Integration]] — MCP tool integration
- [[SAP_Agent_Output_Validation]] — Layer 5 semantic validation detail
- [[SAP_Agent_Multi_Agent]] — HITL for confirmation above threshold
- [[SAP_Agent_Error_Handling]] — error hierarchy and GuardrailError
- [[Enterprise_AI_Architecture]] — enterprise guardrail philosophy
- [[Claude_Code_Hooks]] — Pre-Execution 守卫等价：GuardedMCPToolset 的前置验证与 PreToolUse hook 结构上同构
- [[Prompt_Injection]] — 提示注入攻击（6层防御架构直接针对的核心威胁）

[Source: raw/SAP/guardrails-deep-dive.md, raw/SAP/guardrails-for-mcp-deep-dive.md]
