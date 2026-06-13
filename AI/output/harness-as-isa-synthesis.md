---
date: 2026-05-28
source_notes:
  - "[[Agent_Engineer_Learning_Path]]"
  - "[[Agent_Engineering_Primitives]]"
  - "[[Claude_Advanced_Engineering_Insights]]"
  - "[[Instruction_Sharing]]"
  - "[[Karpathy_Methodology]]"
tags: [synthesis, harness-engineering, knowledge-governance, agent-isa, instruction-sharing]
---

# Harness as Agent ISA — 跨笔记综合

## 综合单元
> 核心笔记：[[Agent_Engineer_Learning_Path]]、[[Agent_Engineering_Primitives]]、[[Claude_Advanced_Engineering_Insights]]、[[Instruction_Sharing]]、[[Karpathy_Methodology]]
> 邻居笔记：[[Agent_Engineer_MOC]]、[[Harness_Engineering_Deep_Dive]]、[[Context_Engineering]]、[[LangGraph_Deep_Agents]]、[[Agent_Engineer_Roadmap]]、[[Production_Agent_Engineering]]、[[Agentic_Loop]]、[[Claude_Code_Subagents]]、[[Agentic_Memory_System]]、[[Multi_Agent_Architecture]]、[[Prompt_Injection]]、[[Self_Evolving_Harness]]、[[Claude_Code_Advanced_Features]]、[[Claude_Code_Hooks]]、[[Claude_Code_Self_Evolving]]、[[Claude_Code_Product_Positioning]]、[[Opus_4_7_Migration]]、[[CLAUDE_md_Best_Practices]]、[[AI_Team_Coding_Practice]]、[[Agent_Harness_Engineering]]、[[Claude_Code_Security]]、[[Agent_Engineer_Mental_Models]]、[[MCP_Production_Decision_Framework]]、[[Harness_Engineering_Advanced]]

## 一致主线

贯穿全部核心笔记的统一论断：**模型不是产品，Harness 才是**。从 Karpathy 的 4 Rules（错误率 41%→11%）到 Agent Engineering Primitives 的 5-Test 框架，从 Agent Engineer Learning Path 的 6 阶段路径到 Instruction Sharing 的单一事实来源，从 Claude Advanced Engineering Insights 的程序化阻挡——所有笔记收敛于同一结论：行为可靠性由**结构性约束（Harness）**保证，而非提示词软约束。工程师的核心工作是设计模型运行的 Harness 环境，模型本身已不是主要瓶颈。

## 内在张力

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Subagents 应当只读，Orchestrator 拥有写操作；单 Agent loop 扩展性远超预期 | [[Agent_Engineering_Primitives]] | Fork 继承将父 session 完整上下文传给 Subagent（含写权限场景隐含其中） | [[Claude_Code_Subagents]] |
| CLAUDE.md 超过 200 行时 Claude 遵守度急剧下降，应保持 60-80 行 | [[Karpathy_Methodology]] | 三级 CLAUDE.md 分层 + AGENTS.md + DECISIONS.md 多文件治理，总指令量远超 200 行 | [[CLAUDE_md_Best_Practices]]、[[Harness_Engineering_Advanced]] |
| 建议通过 symlink 从中央仓库复用 Copilot 指令文件 | [[Instruction_Sharing]] | Claude Code 三级分层（Global/Project/Local）天然支持多项目复用，无需 symlink | [[CLAUDE_md_Best_Practices]] |
| Karpathy Loop：让 Agent 自主迭代优化 SOP，人类睡眠时测试并重写 | [[Karpathy_Methodology]] | Human-in-the-Loop 是生产安全基础设施，高风险操作必须人工审批门控 | [[LangGraph_Deep_Agents]]、[[Agentic_Loop]] |

## 涌现洞察

只有将五篇核心笔记合并审视，才能发现：**"知识治理"本身已成为 Agent 工程的核心原语，但整个知识库没有任何一篇笔记用这个名字来命名它**。单看每篇：Karpathy 谈行为规则、Instruction Sharing 谈指令复用、Agent Engineering Primitives 谈工具设计、Learning Path 谈学习阶段、Claude Advanced Insights 谈记忆固化——各自独立。但跨笔记审视后可发现：CLAUDE.md / AGENTS.md / DECISIONS.md 三文件体系 + Skills + 自进化闭环，实际上构成了一套完整的「**Agent 操作系统指令集架构（ISA）**」——它规定了模型在哪些地方可以做什么、不可以做什么、如何学习、如何跨项目复用。这个架构模式正是因为分布在多篇笔记中、从未被单一视角命名，才只能从网络交汇处涌现。

## 知识缺口

现有笔记详细描述"如何构建 Harness"和"哪些结构值得写入指令文件"，但没有任何笔记回答：**一个 Harness 在什么条件下应当被放弃并重写，而不是继续演进？** 技术债积累到何种程度、遗留 Skill 漂移达到多少比例、context governance 成本超过多少 token，才触发 Harness 的"全局重构"而非局部补丁？

**下一步探索方向**：分析生产 Agent 的 postmortem 数据，建立 Harness 健康度量指标——correction 频率趋势、context rot 速率（第 1 步 vs 第 7 步的有效 token 比率）、5-Test 通过率——与重构触发阈值的映射关系。这将填补从"如何构建"到"何时重构"之间缺失的工程决策框架。
