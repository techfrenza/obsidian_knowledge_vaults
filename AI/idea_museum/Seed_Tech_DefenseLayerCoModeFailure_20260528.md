---
name: SAP 六层防御的双层共模失效
description: 6 层防御架构中第 3 层（Prompt Guardrails）和第 4 层（LLM Processing）依赖同一个 LLM——对抗性 Prompt 可同时绕过这两层，留下 4 层有效防御
type: seed
concept: 防御层共模失效（Co-Mode Failure in Defense Layers）
hook_insight: 你建了 6 层 AI 安全防御，以为每层独立——SAP 生产文档的注脚揭示：第 3 层（Prompt 规则注入）和第 4 层（LLM 执行）共用同一个模型，对抗性输入可以一次绕过两层。6 层防御实际只有 4 层独立防御
wiki_link: "[[SAP_Agent_Guardrails]]"
---

# SAP 六层防御的双层共模失效

## 技术核心逻辑

[[SAP_Agent_Guardrails]] 明确标注：

```
Layer 3: Prompt Guardrails — SOFT（依赖 LLM 遵从规则）
Layer 4: LLM Processing  — SOFT（依赖 LLM 遵从规则）
注意：Layers 3+4 不是独立的，两者均依赖 LLM 服从指令
```

攻击路径：
1. 攻击者在检索内容中注入指令（Prompt Injection）
2. Layer 3 的 XML 规则注入被模型"读到"但也被覆盖
3. Layer 4 的 LLM 处理本身执行了注入指令
4. Layer 3 和 Layer 4 的防御同时失效

真正的独立硬防御只有：
- Layer 1（Input Validation）— 格式校验，无 LLM
- Layer 2（Intent Guardrails）— 黑名单匹配，无 LLM
- Layer 5（Output Validation）— 凭证/PII 脱敏，无 LLM
- Layer 6（Action Guardrails）— 数量限制，无 LLM

## 优缺点对比

**实践意义**
- 写操作 Agent 不能仅依赖 Layer 3+4；必须在 Layer 1/2/5/6 中编码关键业务约束
- `GuardedMCPToolset` 的前置验证必须是确定性逻辑（非 LLM judge），才算真正独立防御
- [[Prompt_Injection]] 中描述的 Channel Separation（XML 标记隔离不可信内容）是唯一能减轻 Layer 3+4 共模失效的缓解措施

**系统性启示**
- 任何安全层计数，应先问："这层的判断逻辑是否依赖我要防御的那个组件？"
- "N 层防御"需要报告"其中独立层数 M"，而非仅报告总层数

[Source: wiki/SAP_Agent_Guardrails, wiki/Production_Agent_Engineering]
