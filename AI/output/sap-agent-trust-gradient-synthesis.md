---
date: 2026-05-28
source_notes:
  - "[[SAP_Agent_Cards]]"
  - "[[SAP_Agent_Code_Quality]]"
  - "[[SAP_Agent_Durable_Execution]]"
  - "[[SAP_Agent_Error_Handling]]"
  - "[[SAP_Agent_Evaluation]]"
  - "[[SAP_Agent_Guardrails]]"
  - "[[SAP_Agent_Guardrails_MCP]]"
  - "[[SAP_Agent_Joule_Integration]]"
  - "[[SAP_Agent_LangGraph]]"
  - "[[SAP_Agent_MCP_Integration]]"
  - "[[SAP_Agent_Memory_Service]]"
  - "[[SAP_Agent_Multi_Agent]]"
  - "[[SAP_Agent_ORD_Registration]]"
  - "[[SAP_Agent_Output_Validation]]"
  - "[[SAP_Agent_Overview]]"
  - "[[SAP_Agent_Performance]]"
  - "[[SAP_Agent_Prompt_Engineering]]"
  - "[[SAP_Agent_Resilience]]"
  - "[[SAP_Agent_Ship_Checklist]]"
  - "[[SAP_Agent_Skills]]"
tags: [synthesis, sap-agents, enterprise-ai, trust-gradient, write-safety]
---

# SAP Agent Trust Gradient — 跨笔记综合

## 综合单元

> 核心笔记：[[SAP_Agent_Cards]]、[[SAP_Agent_Code_Quality]]、[[SAP_Agent_Durable_Execution]]、[[SAP_Agent_Error_Handling]]、[[SAP_Agent_Evaluation]]、[[SAP_Agent_Guardrails]]、[[SAP_Agent_Guardrails_MCP]]、[[SAP_Agent_Joule_Integration]]、[[SAP_Agent_LangGraph]]、[[SAP_Agent_MCP_Integration]]、[[SAP_Agent_Memory_Service]]、[[SAP_Agent_Multi_Agent]]、[[SAP_Agent_ORD_Registration]]、[[SAP_Agent_Output_Validation]]、[[SAP_Agent_Overview]]、[[SAP_Agent_Performance]]、[[SAP_Agent_Prompt_Engineering]]、[[SAP_Agent_Resilience]]、[[SAP_Agent_Ship_Checklist]]、[[SAP_Agent_Skills]]
>
> 邻居笔记：[[Agentic_Loop]]、[[Agentic_Memory_System]]、[[AI_Team_Coding_Practice]]、[[Agent_Governance_Layers]]、[[Agent_Payments_Risk_Matrix]]、[[Bending_Spoons_Universal_OS]]、[[Claude_Code_Hooks]]、[[Claude_Code_Skills]]、[[Context_Engineering]]、[[Cross_Platform_Memory]]、[[Enterprise_AI_Architecture]]、[[Enterprise_Agent_Playbook]]、[[Forward_Deployed_Engineering]]、[[GBrain_Architecture]]、[[Hermes_Agent]]、[[Human_In_The_Loop]]、[[LangGraph_Build_Agents]]、[[LangGraph_Deep_Agents]]、[[MCP_Enterprise_Integrations]]、[[MCP_Integration_Playbook]]、[[MCP_Production_Agent]]、[[MCP_Production_Decision_Framework]]、[[Managed_Agent_Memory]]、[[MultiAgent_Concurrent_Write_Research]]、[[Multi_Agent_Architecture]]、[[Production_Agent_Engineering]]、[[Production_Reliability_MOC]]、[[Prompt_Engineering_Advanced]]、[[Prompt_Injection]]、[[PydanticAI]]、[[SAP_Agent_Testing]]、[[SAP_Agent_UMS_Registry]]、[[Security_MOC]]、[[Skill_Design_Patterns]]、[[Skill_Ecosystem]]、[[Tokenmaxxing]]、[[Unique_Engineering_Insights]]

---

## 一致主线

贯穿全部 20 篇笔记的核心论断只有一条：**用显式、可验证的分层约束替代 LLM 的涌现行为**。这一原则在每个子领域均有具体实现：YAML 外部化的硬性护栏覆盖软性提示规则（[[SAP_Agent_Guardrails]]）；三判决模式在 SAP 写入前执行语义校验（[[SAP_Agent_Output_Validation]]）；约束代理哲学以逐步骤指令取代开放式工具调用（[[SAP_Agent_Evaluation]]）；Harness 驱动的强制技能注入优先于模型自主激活（[[SAP_Agent_Skills]]）；Single-Execution Guard 持久化到 LangGraph AgentState 以保证幂等性（[[SAP_Agent_LangGraph]]）。[[SAP_Agent_Overview]] 的 13 步生产路径是这一原则的架构化编码。

---

## 内在张力

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| "更多自主权是长期目标——通过验证过的生产行为逐步赢得" | [[SAP_Agent_Evaluation]] | 模型驱动技能激活"灵活但不可预测——不推荐用于企业写操作" | [[SAP_Agent_Skills]] |
| 跨 Provider 降级（Anthropic→GPT-5）即是成本优化策略（节省 60%），也是可靠性保障 | [[SAP_Agent_Prompt_Engineering]] | 写操作代理必须**永远不**从 capable 降级到 fast，若所有 capable 模型均失败则干净报错 | [[SAP_Agent_Resilience]] |
| TR11 强制要求所有工具通过 MCP Server 暴露和消费 | [[SAP_Agent_Ship_Checklist]] | MCP 工具由 API spec 生成并跨 Agent 共享，因此**护栏不能放在 MCP Server 侧** | [[SAP_Agent_Guardrails_MCP]] |
| 约束代理：显式步骤指令 = 更确定性、可测试的行为（推荐当前） | [[SAP_Agent_Evaluation]] | 约束代理是临时阶段，最终目标是完全自主 Agent（长期方向） | [[SAP_Agent_Evaluation]] |

---

## 涌现洞察

**整个 SAP Agent 技术栈是一个形式化的信任梯度（Trust Gradient）**：操作越靠近 SAP 写事务（不可逆财务过账），硬性约束对软性（依赖 LLM）控制的替代程度就越高。

这条梯度从软到硬依次为：
1. `SemanticFieldSelector`（软，相似度阈值 0.3）→ 语义字段选择
2. YAML 护栏 XML 注入（软，提示层，对抗性输入可绕过）
3. `AmountLimitRule` / `GuardedMCPToolset`（硬，执行前拦截）
4. `Single-Execution Guard` + `_called_write_tools`（硬，状态持久化）
5. LiteLLM Router `fallbacks=[]` for write agents（硬，基础设施级）
6. Temporal durable execution + checkpoint（硬，跨重启恢复）

这条梯度**在任何单篇笔记中都未被命名或可视化**——它只有在将全部 20 篇放在一起阅读时才能浮现。它解释了为什么"约束代理"不是哲学选择，而是工程必然：SAP 写操作的财务后果要求每层防御都独立于 LLM 的服从性。

---

## 知识缺口

**未被任何笔记回答的关键问题：** 谁拥有并治理那些定义信任梯度"阈值"的参数？

具体表现：
- `guardrails/config.yaml` 中的 `max_amount: 1000000`、`confirm_threshold: 10000` 由谁审批、版本化、审计？
- `guardrails/roles/accountant.yaml` 中的 `max_amount: 50000` 如何与 SAP 授权概念（SoD）对齐？
- 护栏阈值变更是否等同于代码变更（需要 PR review + 测试），还是可以绕过工程流程？

对比：TR13 Agent Step 定价层级明确由"LoB / Controlling / BAI / BMP 联合决定"（[[SAP_Agent_Ship_Checklist]]）——这是对业务参数治理有意识的制度设计。但护栏阈值没有对等机制。

**下一步探索方向：** 调研是否存在"配置即代码"（Config-as-Code）的护栏治理模式——将 YAML 护栏文件纳入与业务规则变更审批（如 SAP Change Management）挂钩的审批流，并为每次阈值变更生成可查审计日志。这是从"技术安全"到"合规安全"的关键跨越。
