---
date: 2026-05-28
source_notes:
  - "[[SAP_Agent_Testing]]"
  - "[[SAP_Agent_UMS_Registry]]"
  - "[[Security_MOC]]"
  - "[[Self_Evolving_Harness]]"
  - "[[Seven_Agent_Software_Factory]]"
  - "[[Skill_Design_Patterns]]"
  - "[[Skill_Ecosystem]]"
  - "[[Skill_Engineering_10_Rules]]"
  - "[[Solo_Founder_3_Agent_System]]"
  - "[[Solo_Founder_Agent]]"
  - "[[Tokenmaxxing]]"
  - "[[Unique_Engineering_Insights]]"
tags: [synthesis, skill-engineering, harness-coevolution, production-agent, self-improving-systems]
---

# Skill-Harness Co-Evolution — 跨笔记综合

## 综合单元

> 核心笔记：[[SAP_Agent_Testing]]、[[SAP_Agent_UMS_Registry]]、[[Security_MOC]]、[[Self_Evolving_Harness]]、[[Seven_Agent_Software_Factory]]、[[Skill_Design_Patterns]]、[[Skill_Ecosystem]]、[[Skill_Engineering_10_Rules]]、[[Solo_Founder_3_Agent_System]]、[[Solo_Founder_Agent]]、[[Tokenmaxxing]]、[[Unique_Engineering_Insights]]
>
> 邻居笔记：[[AI_Agent_247_Architecture]]、[[AI_Agent_Payments]]、[[AI_Native_Startup_Playbook]]、[[AI_Native_Tool_Design]]、[[AI_OS_Framework]]、[[AI_Team_Coding_Practice]]、[[AI_Workflow_System]]、[[Agent_Engineer_Mental_Models]]、[[Agent_Engineer_Roadmap]]、[[Agent_Engineering_Primitives]]、[[Agent_Harness_Engineering]]、[[Agentic_Memory_System]]、[[Anthropic_Agent_SDK]]、[[CLAUDE_md_Best_Practices]]、[[Claude_Advanced_Engineering_Insights]]、[[Claude Code Commands Reference]]、[[Claude_Code_Hooks]]、[[Claude_Code_MOC]]、[[Claude_Code_Product_Positioning]]、[[Claude_Code_Routines]]、[[Claude_Code_Security]]、[[Claude_Code_Self_Evolving]]、[[Claude_Code_Skills]]、[[Claude_Code_Subagents]]、[[Context_Engineering]]、[[Contextmaxxing]]、[[Enterprise_AI_Architecture]]、[[Enterprise_Agent_Playbook]]、[[GBrain_Architecture]]、[[GBrain_Fat_Thin_Architecture]]、[[Harness_Engineering_Advanced]]、[[Harness_Engineering_Deep_Dive]]、[[Harness_Over_Model_Principle]]、[[Human_In_The_Loop]]、[[Institutional_Evolution_Flywheel]]、[[LangGraph_Build_Agents]]、[[MCP_Integration_Playbook]]、[[MCP_Production_Agent]]、[[Metaprompting]]、[[Multi_Agent_Architecture]]、[[Multi_Agent_Missions_System]]、[[Production_Agent_Engineering]]、[[Production_Reliability_MOC]]、[[Prompt_Engineering_Library]]、[[Prompt_Engineering_MOC]]、[[Prompt_Injection]]、[[Prompt_Template_Library]]、[[PydanticAI]]、[[RLM_Simulation]]、[[SAP_Agent_Cards]]、[[SAP_Agent_Error_Handling]]、[[SAP_Agent_Evaluation]]、[[SAP_Agent_Guardrails]]、[[SAP_Agent_Guardrails_MCP]]、[[SAP_Agent_Joule_Integration]]、[[SAP_Agent_ORD_Registration]]、[[SAP_Agent_Overview]]、[[SAP_Agent_Performance]]、[[SAP_Agent_Prompt_Engineering]]、[[SAP_Agent_Ship_Checklist]]、[[SAP_Agent_Skills]]

---

## 一致主线

跨越本批所有笔记，一条统一论断反复出现：**Skills 是 Harness 的基本编译单元，而非附加插件**。从 [[Skill_Engineering_10_Rules]] 的"确定性代码优于概率性 LLM"、到 [[Self_Evolving_Harness]] 的"Harness 需要像软件一样版本化和自进化"、再到 [[SAP_Agent_Testing]] 的"五层测试金字塔 + TestModel 让大多数测试无需真实 LLM"——这三个来源共同指向同一结论：**产品质量的决定变量不是模型，而是 Skill 的工程严密度与 Harness 的自我演化能力**。[[Tokenmaxxing]] 和 [[Unique_Engineering_Insights]] 的实证数据（同一模型在不同 Harness 下任务通过率 78% vs 42%）在量化层面为此提供了闭环支撑。[[Solo_Founder_3_Agent_System]] 和 [[Seven_Agent_Software_Factory]] 则展示了当 Skill 工程达到足够严密度时，小团队甚至单人创业者可以实现"Agent 替代员工"的规模化效果。

---

## 内在张力

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Skills 应尽量"薄"：只写触发器和步骤，核心逻辑用确定性脚本（.mjs）实现，避免让 LLM 猜测控制流 | [[Skill_Engineering_10_Rules]]（规则 2） | Tokenmaxxing 鼓励"烧 Token"：把所有相关 Context（代码、文档、历史 PR）一次性塞满，先让 Agent "Boil the Ocean"再处理细节 | [[Tokenmaxxing]]（Step 2） |
| SAP Agent Testing 提倡用 `TestModel`（无 LLM，100% 确定性）覆盖金字塔底部 3 层，只有 E2E 和 Eval 层才允许真实模型 | [[SAP_Agent_Testing]]（五层金字塔） | Skill_Engineering_10_Rules 的规则 5 要求引入"LLM-as-Judge"（Haiku 作裁判）评估主观输出，此层无法避免 LLM | [[Skill_Engineering_10_Rules]]（规则 5） |
| Security_MOC 核心原则：程序化阻挡（Hooks/Guardrails）优于提示词软约束，安全控制必须前移到系统层 | [[Security_MOC]]、[[SAP_Agent_Guardrails]] | Self_Evolving_Harness 的自进化机制依赖 LLM 对生产日志进行分类和规则蒸馏（dreaming 机制），这本身是概率性 LLM 操作 | [[Self_Evolving_Harness]]（Core Paradox） |
| Seven_Agent_Software_Factory 设定 Validator Agent 只报告差距、不做修改（只读权限），作为最终质量门 | [[Seven_Agent_Software_Factory]] | Solo_Founder_3_Agent_System 的 Content Agent 自动评分低于阈值即自动重写，质量门内嵌于 Agent 循环中，无静态 Validator 角色 | [[Solo_Founder_3_Agent_System]] |

---

## 涌现洞察

**"自进化"不是终点，而是 Skill 生命周期的第三阶段**：当从网络视角审视 [[Self_Evolving_Harness]]、[[Institutional_Evolution_Flywheel]]、[[Skill_Engineering_10_Rules]] 和 [[SAP_Agent_UMS_Registry]] 时，浮现出一个单篇笔记无法呈现的完整周期——(1) **设计阶段**：Skill 以确定性脚本为核心，SKILL.md 作契约（Skill_Engineering_10_Rules），(2) **注册阶段**：Skill/Agent 通过 ORD 端点暴露元数据进入 UMS 目录（SAP_Agent_UMS_Registry），(3) **自进化阶段**：生产错误经 Harness 蒸馏后回写为新规则，更新 SKILL.md 或 Harness 约束（Self_Evolving_Harness + Institutional_Evolution_Flywheel）。

这个三阶段循环只有在把"运维（UMS/Registry）"与"工程（Skill 设计）"与"演化（Flywheel）"三个通常独立叙述的维度放到同一视角才能发现——**Agent 注册 ≠ 部署终态，而是进化闭环的输入采集器**。SAP_Agent_UMS_Registry 的"system-instance 动态注册"实际上是 Institutional Evolution Flywheel 的数据馈入点，而非孤立的 ServiceMesh 配置。

---

## 知识缺口

**尚未回答的关键问题**：当 Harness 的自进化机制（[[Self_Evolving_Harness]] 的 dreaming、[[Claude_Code_Self_Evolving]] 的 Corrections→Rules 闭环）产生的新规则与企业级注册系统（[[SAP_Agent_UMS_Registry]] 的 system-instance）中记录的 Agent 行为规格产生冲突时，**谁拥有最终权威（Source of Truth）**？本批笔记的每一篇都在各自的范围内定义了规则更新机制，但没有任何一篇描述跨层级（Harness 规则 vs 注册中心规格 vs 企业安全策略）的冲突解决协议。

**下一步探索建议**：调研 SAP BTP 环境中 Harness 自进化生成的新约束如何经 ORD 变更通知传播到 UMS，再由 Joule Studio / Agent Gateway 感知——即"规则版本化与跨系统传播"是否已有标准实现，或是当前的工程盲区。可从 [[SAP_Agent_Ship_Checklist]] 和 [[SAP_Agent_ORD_Registration]] 入手，检索是否存在"behavior contract versioning"的描述。
