# Wiki Index

> 最后更新：2026-06-09（第二十八次编译）| 笔记总数：123 | 来源：~222 个 raw 文件

---

## Part 0：知识地图 MOC (Maps of Content)

- [[Agent_Engineer_MOC]] — Agent Engineer 体系：心智模型→Roadmap→Loop→SDK→Harness
- [[Claude_Code_MOC]] — Claude Code 生态全图：12 页体系、架构层级、延伸路径
- [[Production_Reliability_MOC]] — 生产可靠性：247架构/三层分离/HITL/安全防线
- [[Memory_MOC]] — 记忆体系 MOC
- [[Security_MOC]] — 安全体系：攻击面/防御架构/运行时控制 **[NEW]**

---

## Part 1：记忆与上下文 (Memory & Context)

- [[Agent_Context_Architecture]] — 企业 Agent 四层 Context 分层（Episodic/Semantic/Procedural/Working）
- [[Agentic_Memory_System]] — 四类内存架构（In-context/External/Episodic/Parametric）+ 代码模板 + **Memory+Skills同一世界模型原则**
- [[Knowledge_Graph_Memory]] — Pydantic Schema控制的知识图谱记忆：多跳推理/本体设计/10-10-10约束 **[NEW]**
- [[Cross_Platform_Memory]] — 跨 AI 平台 Markdown 记忆迁移系统
- [[Managed_Agent_Memory]] — Anthropic 官方 Memory Store API
- [[Claude_Memory_Layers]] — 三层记忆系统（原生 Settings/桌面文件系统/Obsidian Wiki）
- [[Context_Engineering]] — Context is State、四大原语（Write/Select/Compress/Isolate）、三层框架、四文件架构
- [[Contextmaxxing]] — 最大化每 Token 的相关上下文质量（vs Tokenmaxxing 最大化用量）、企业记忆基础设施

---

## Part 2：Harness 工程 (Harness Engineering)

- [[Agent_Harness_Engineering]] — 核心 Orchestration Loop、三维度 Scaling、七架构决策、容错恢复层级
- [[Harness_Over_Model_Principle]] — Harness 重于模型的核心公理：实证数据（78% vs 42%）+ 三层制度控制平面 **[NEW]**
- [[Harness_Engineering_Deep_Dive]] — 定义、5 大方法 + **12组件生产架构 + 七决策树 + Anti-Rot 三层持久化**（Evaluator-Optimiser/Physical Blocks/渐进自主权）
- [[Harness_Engineering_Advanced]] — Harness 进阶指南：三层持久化上下文/Plan-First 工作流/并行编排/熔断机制 **[NEW]**
- [[MCP_Production_Agent]] — MCP vs API vs CLI 决策树、context-efficient 模式
- [[MCP_Production_Decision_Framework]] — 三选决策树、MCP Server 构建模式、Elicitation、配对插件
- [[MCP_Connectors]] — 官方 Connectors UI 设置、顶级 12 个 MCP 工具、MCP Hub Project 模式
- [[MCP_Integration_Playbook]] — 12 工具实战清单、MCP Hub Project 设置模板、Vibe-Code Dashboard
- [[MCP_Enterprise_Integrations]] — 企业 MCP（Microsoft Teams + JIRA）、Azure AD 应用注册、Atlassian OAuth
- [[AI_Native_Tool_Design]] — 为 AI 重新设计工具（非薄封装）：无记忆/不浏览/需要精确三大约束 + Cloudflare 2-Tool 案例 **[NEW]**
- [[Generative_UI_Architecture]] — AG-UI/A2UI 协议栈 + 三种 GenUI 模式（Controlled/Declarative/Open-ended）+ 决策树 **[NEW]**
- [[AI_Orchestration_System]] — 100x 工具栈、Plan-First 三阶段、Night Queue
- [[AI_Orchestration_Practice]] — 5 层工具栈、Parallel Agents、Persistent Context 系统
- [[Enterprise_AI_Architecture]] — 企业 MCP 三层架构、LangGraph、Guardian Agents、Evals-Driven
- [[Multi_Agent_Architecture]] — 三层分离（Skills/Agents/MCP）、安全分层隔离、Handoff 模式 + **Factory Missions 5种协作模式 + Validation Contract**
- [[MultiAgent_Concurrent_Write_Research]] — 多 Agent 并发写入上下文资产的冲突问题：CRDT/乐观锁/主写者架构四种候选解法 **[NEW]**
- [[Multi_Agent_Missions_System]] — Factory Missions 详解：Orchestrator/Workers/Validators 三角色/Validation Contract First/Droid Whispering **[NEW]**
- [[Seven_Agent_Software_Factory]] — 7角色软件工厂：Researcher/StoryWriter/Spec/Backend/Frontend/Tester/Validator + repo级Harness目录结构 **[NEW]**
- [[AI_Team_Coding_Practice]] — AGENTS.md/DECISIONS.md 上下文资产、Plan→Compound 闭环
- [[Human_In_The_Loop]] — HITL 工具调用拦截钩子、确定性门禁、高风险操作拦截
- [[Agent_Governance_Layers]] — 5层治理控制平面：Intent Boundary/Permission Model/Audit Trail/Escalation Protocol/Feedback Loop **[NEW]**
- [[Forward_Deployed_Engineering]] — FDE角色：Audit/Evals/Deployment三阶段方法论 + 30天成为FDE路径 **[NEW]**
- [[Solo_Founder_Agent]] — 3 个专业 Agent 替代 70-80% 全职工作 + **6 营销 Agent 替换方案 + 30天创业路径**
- [[Solo_Founder_3_Agent_System]] — Research/Content/Operations 三 Agent 协同：共享知识库/质量门控/3周落地 **[NEW]**
- [[AI_Agent_247_Architecture]] — 3 大生存规则（Job Description/Visibility/托管运行）、主流方案对比
- [[AI_Agent_Payments]] — x402协议/USDC/M2M支付/Bedrock AgentCore Payments/Uniswap AI Suite
- [[Agent_Payments_Risk_Matrix]] — 三层支付风险决策矩阵：只读自主/小额自动/高风险HITL **[NEW]**
- [[LangGraph_Deep_Agents]] — LangGraph 有状态运行时、Deep Agents 组件包、五种工作流模式、AP2 协议
- [[LangGraph_Build_Agents]] — State/Nodes/Edges 架构、Evaluator-Optimizer 模式、记忆分层
- [[Hermes_Agent]] — 开源自进化 Agent：5 大支柱（Memory/Skills/Soul/Crons/Self-Improving Loop）、VPS 部署、vs Claude Code
- [[Self_Evolving_Harness]] — Harness 自进化：Tracing为核心/Error Pattern巡检案例/L1-L4进化层次/Consumer产品护城河 **[NEW]**
- [[AI_Native_Startup_Playbook]] — Anthropic 官方创业四阶段（Idea/MVP/Launch/Scale）+ Agentic技术债复利 + PMF 陷阱 + 工作流锁定护城河 + **三大反直觉洞察：AI镜像偏误/Build越便宜Validation越贵/CLAUDE.md复利账户 [UPDATED]**
- [[Bending_Spoons_Universal_OS]] — Bending Spoons分层中央平台（Universal OS）+ 6自研模块 + Multi-Agent代码迁移框架（M/E/T-Agent三循环）**[NEW]**
- [[Enterprise_Agent_Playbook]] — 六个企业级落地蓝图：Continuous Audit / Manager Amplification / Builders-Measurers / AI-Native招聘 / 自进化闭环 / AI转型咨询
- [[AI_Native_Engineering_Org]] — AI-Native 工程组织变革：JIT规划/代码不再是瓶颈/角色模糊化/瓶颈从写代码转移到验证 **[NEW]**
- [[Enterprise_Agentic_AI_6_Ideas]] — 企业Agentic AI 6大落地方案详解：审计/管理扁平化/组织分析/招聘/闭环工程/咨询模板 **[NEW]**
- [[Production_Agent_Engineering]] — 生产Agent四大原语：Token经济/Skill组合/能力安全/信任遥测 **[NEW]**
- [[Agent_Engineering_Primitives]] — 5-Test框架过滤新工具/持久原语(Context/Tool Design/Orchestrator-Subagent/Evals/File-System-State)/框架选型2026 **[NEW]**
- [[Agent_Engineer_Learning_Path]] — 6阶段学习Roadmap + 三大心智模型 + LangGraph vs Claude SDK双轨技术栈 + 框架Graveyard **[NEW]**

---

## Part 3：Claude Code 生态 (Claude Code Ecosystem)

- [[CLAUDE_md_Best_Practices]] — 60-80 行规则文件写法、三级配置架构、**Karpathy 12 规则系统 + Anthropic/Shopify 内部模板 + Interview Pattern + 21规则3层系统+ERRORS.md机制 [UPDATED]**
- [[Claude_Code_Skills]] — Skill 六大模式 + **Skill 文件夹结构 + 三层成本 + Eval-First + Gotcha节** + Karpathy Loop + **教育内容→Skill 转换**
- [[Claude code CLI -running 2 skills in background and front]] — Claude Code CLI 并行技能运行：5种模式（Ctrl+B/SKILL.md fork/Git Worktrees/loop+batch/监控防死循环）**[NEW]**
- [[Skill_Design_Patterns]] — 五大 SKILL.md 设计模式（Tool Wrapper/Generator/Reviewer/Inversion/Pipeline）
- [[Skill_Ecosystem]] — Skills 生态资源地图：47 精选工具/MCP 服务器/Agent 框架/多 Agent 编排
- [[Skill_Engineering_10_Rules]] — 生产级 Skill 十大规则：SKILL.md 契约/确定性代码/单测/集成测/Evals/Resolver/DRY/E2E **[NEW]**
- [[Claude_Code_Hooks]] — 确定性执行层、postEdit/pre_edit/on_failure 模板、Hooks vs Skills 核心区分
- [[Claude_Code_Subagents]] — 上下文隔离、Fork 继承、Parallel Agents + **7 生产级角色模板 + Subagents vs Agent Teams 精确区分 + 上下文中心设计原则 [UPDATED]**
- [[Claude_Code_Settings]] — settings.json 权限管理、安全红线
- [[Claude_Code_Security]] — .env 保护、全局 deny 规则、pre-commit hook 模板
- [[Prompt_Injection]] — 提示注入攻击分类（直接/间接/多轮渐进/策略伪装）+ 分层防御策略 **[NEW]**
- [[Claude_Code_Routines]] — 云端自动化、Schedule/API/GitHub Trigger
- [[Claude_Code_Commands]] — 35 个日常命令、六大心智模型
- [[Claude_Code_CLI_Reference]] — CLI 命令/flags/斜杠命令完整参考 + Effort层级 + 权限模式速查 **[NEW]**
- [[Claude_Code_Self_Evolving]] — 四层认知架构、Corrections→Rules→Verification 循环
- [[Generator_Evaluator_Separation]] — Generator 与 Evaluator 隔离的三种方案（CLAUDE.evaluator.md / /clear+角色切换 / Worktree+新会话）**[NEW]**
- [[Claude_Code_Advanced_Features]] — CLAUDE.md/Skills/权限模式/Computer Use/Cloud Routines/Audit + **学术研究者工作流 + 大型代码库7组件Harness + Anthropic内部7步循环**
- [[Claude_Code_Product_Positioning]] — Agentic System 三层架构、9 层安全防御、KAIROS + **五级进阶路径 + 7天上手路径**
- [[Claude_Advanced_Engineering_Insights]] — KAIROS守护进程/dream记忆固化/Skeptical Evaluator/反蒸馏防御/错误主路径设计/程序化阻挡原则 **[NEW]**
- [[Claude_Optimization]] — 8 大实战修复（工具/模型/Prompt/XML Tags/Context Rot）
- [[Claude_Cowork]] — Claude Cowork 平台：Plugins/Connectors/Slash Commands/Sub-agents 四层架构
- [[Claude_Projects_Power_Usage]] — Claude Projects 25技巧：永久System Prompt/知识库上传/Living Instructions/Compounding Knowledge **[NEW]**
- [[Instruction_Sharing]] — 多项目共享 Copilot 指令文件：symlink（Linux/macOS）+ NTFS junction（Windows）
- [[Claude_Code_Hacks]] — 32 个 Beginner/Intermediate/Pro 技巧（Ultrathink/Worktrees/Agent Teams/Loop）

---

## Part 4：Agent 工程师体系 (Agent Engineer)

- [[Agent_Engineer_Roadmap]] — 6 阶段 17 周路径、两大核心栈、**5-Filter 测试、Q3 2026 观察清单**
- [[Agent_Engineer_Mental_Models]] — Workflow vs Agent、增强型 LLM、上下文原语三大心智模型
- [[Agent_Engineer_Three_Mental_Models]] — 三大心智模型详解：Workflow vs Agent / Augmented LLM / Context Primitives + MCP三原语 **[NEW]**
- [[Agent_Engineer_Core_Stacks]] — 两大核心栈：LangGraph + Claude SDK + 6阶段17周完整Roadmap **[NEW]**
- [[Anthropic_Agent_SDK]] — 代理循环四阶段、子代理系统、Hooks 机制、权限模型
- [[Agentic_Loop]] — 代理循环 vs 传统工作流对比、成本风险、HITL 建议
- [[Loop_Engineering]] — Loop 工程学：DISCOVER→PLAN→EXECUTE→VERIFY→ITERATE + Open/Closed Loop + 6构建块 + 14步路线图 **[NEW]**
- [[Unique_Engineering_Insights]] — Harness > 模型实证、Dreaming/KAIROS/Anti-distillation、Skeptical Evaluator

---

## Part 5：进阶提示工程与方法论

- [[Prompt_Engineering_MOC]] — 提示工程知识地图：模板库/元提示/研究写作工作流全系索引 **[NEW]**
- [[Karpathy_Methodology]] — Karpathy 方法论：4+8 规则（错误率41%→11%）/Karpathy Loop/LLM Wiki/AIOS四个C **[NEW]**
- [[Metaprompting]] — 元提示工程：四步迭代循环/Bookmarkability Rubric/v1→v10 超级提示进化 **[NEW]**
- [[Opus_4_7_Migration]] — 4.7 迁移指南：adaptive thinking、xhigh effort
- [[RLM_Simulation]] — 递归语言模型模拟、Context Rot 防治
- [[Prompt_Engineering_Library]] — 40 个专家级 Prompt 模板（Writing/Analysis/Technical/Communication）
- [[Prompt_Template_Library]] — 40 个即用模板完整列表（按类别分组，含 `[变量]` 替换）
- [[Prompt_Engineering_Advanced]] — Metaprompting 进化循环 + Prompt Folding（Classifier→Sub-Prompt）
- [[Research_Prompts]] — 4 步研究写作工作流（论点提取/敌对审稿/Steelman/24h 提升）
- [[Tokenmaxxing]] — 不省 Token 策略：Boil the Ocean + RAG + Hybrid Search 实现 400x 产出；六大心智模型

---

## Part 6：工作流与产品平台 (Workflow & Products)

- [[AI_Workflow_System]] — 5 阶段业务流程自动化框架：Workflow-First + 三层记忆系统
- [[AI_OS_Framework]] — Four Cs 框架（Context/Connections/Capabilities/Cadence）+ 42 条实战 Tips
- [[GBrain_Architecture]] — Garry Tan 个人 AI 大脑：Fat Skills + Thin Harness + 100k 页知识图谱 + Skillify 10步 + Auto-think/Auto-build 循环
- [[GBrain_Fat_Thin_Architecture]] — GBrain 架构详解：Meeting Skill/Book-Mirror/Skillify/7天启动SOP **[NEW]**

---

## Part 7：SAP Agent Engineering

- [[SAP_Agent_Overview]] — 全局 MOC：PydanticAI + LiteLLM + AppFND + A2A 技术栈、13步生产路径
- [[SAP_Agent_Prompt_Engineering]] — 七层 PromptBuilder、外部化 YAML、结构化输出、Few-Shot、幻觉缓解、模型选择
- [[SAP_Agent_Multi_Agent]] — A2AClient、AgentContext、四种编排模式（Sequential/Parallel/Router/Orchestrator）、HITL
- [[SAP_Agent_MCP_Integration]] — MCP作为OData抽象层、SemanticFieldSelector、FieldMapper、DestinationServiceClient
- [[SAP_Agent_Guardrails]] — 六层防御、YAML配置、GuardedMCPToolset、GuardrailAuditLogger
- [[SAP_Agent_Guardrails_MCP]] — GuardedMCPToolset 中间件：agent侧注入 per-agent 规则、EnforceableRule 接口、AmountLimitRule **[NEW]**
- [[SAP_Agent_Resilience]] — CircuitBreaker、LiteLLM Router、写代理安全矩阵、Bulkhead、分层超时
- [[SAP_Agent_Error_Handling]] — 异常层次、AgentLoopController、DeadLetterQueue、RetryPolicy
- [[SAP_Agent_Output_Validation]] — Three-Verdict Pattern、Single-Execution Guard、LangGraph节点放置
- [[SAP_Agent_Testing]] — 五层测试金字塔、TestModel、Aeval框架（correctness 0.4/helpfulness 0.3/safety 0.2）
- [[SAP_Agent_Performance]] — 批处理优先、TieredPromptManager、MultiLayerCache（GLAccount TTL=3600/余额TTL=0）
- [[SAP_Agent_Skills]] — SKILL.md标准（agentskills.io）、SkillLifecycleManager、三种激活模式
- [[SAP_Agent_Cards]] — Agent Card JSON Schema、命名规范 pc-{domain}-{function}-agent、AgentCardValidator
- [[SAP_Agent_LangGraph]] — LangGraph节点/状态/边、HITL interrupt()、PostgresSaver、OutputValidator集成
- [[SAP_Agent_Durable_Execution]] — LangGraph vs Temporal vs DBOS决策指南、三大设计模式
- [[SAP_Agent_Memory_Service]] — Episodic/Semantic/Procedural三类记忆、SAP HANA Cloud存储
- [[SAP_Agent_ORD_Registration]] — ORD端点（TR6强制）、sap-agent-ord-endpoint技能、CPA命名空间
- [[SAP_Agent_UMS_Registry]] — Unified Metadata Service、system-version vs system-instance、数据流
- [[SAP_Agent_Joule_Integration]] — Agent Gateway、IAS App2App双向授权、Joule设计时构件
- [[SAP_Agent_Ship_Checklist]] — TR1–TR14技术要求、Agent Step计费、PM门控
- [[SAP_Agent_Code_Quality]] — Vibe Code Reviewer（9类检查）、God File Decomposer、Anti-Patterns
- [[SAP_Agent_Evaluation]] — Testing Onion（3层）、Constrained Agency哲学、aeval框架

---

## Meta / 管理

- [[log]] — Wiki 编译历史日志

---

## 编译日志

- 2026-06-09（第二十八次编译）：编译 4 个指定 raw 文件。新增 2 页（Loop_Engineering/Generator_Evaluator_Separation）。更新 1 页（Claude_Code_Routines：新增 Cowork Workflow 框架）。跳过 1 个（Claude Project 25 Features 已覆盖）。笔记总数：123。
- 2026-06-05（第二十六次编译）：编译 21 个新 raw 文件。新增 3 页（AI_Native_Engineering_Org/Generative_UI_Architecture/Claude_Projects_Power_Usage）。更新 4 页（Context_Engineering/Multi_Agent_Architecture/Claude_Code_Subagents/index.md）。跳过文件 ~14 个（重复/来源已覆盖）。笔记总数：120。
- 2026-05-28（第二十四次编译）：编译 8 个新 raw 文件。新增 6 页（Agent_Governance_Layers/Self_Evolving_Harness/Production_Agent_Engineering/AI_Native_Tool_Design/Knowledge_Graph_Memory/Forward_Deployed_Engineering）。更新 3 页（Agentic_Memory_System/Multi_Agent_Architecture/index.md）。新增 3 个 idea_museum seeds。笔记总数：113。
- 2026-05-27（Lint Apply）：执行 Linting_Report_2026-05-27_2200 修复。收录 4 个缺失笔记（Harness_Over_Model_Principle/MultiAgent_Concurrent_Write_Research/Claude code CLI -running 2 skills/SAP_Agent_Guardrails_MCP）。补全 SAP_Agent_Overview MOC。消除 8 个文件共 63 条重复链接。笔记总数：106。
- 2026-05-27（第二十二次编译）：编译 12 个新 raw 文件。新增 4 页（Bending_Spoons_Universal_OS/Prompt_Injection/Agent_Payments_Risk_Matrix/Seven_Agent_Software_Factory）。更新 7 页（Harness_Engineering_Advanced/AI_Agent_Payments/Institutional_Evolution_Flywheel/SAP_Agent_MCP_Integration/MCP_Production_Agent/Claude_Code_Advanced_Features/Solo_Founder_Agent）。新增 3 个 idea_museum seeds。笔记总数：105。
- 2026-05-24（第二十次编译）：编译 10 个新 raw 文件。新增 10 页（Skill_Engineering_10_Rules/Harness_Engineering_Advanced/GBrain_Fat_Thin_Architecture/Karpathy_Methodology/Metaprompting/Multi_Agent_Missions_System/Solo_Founder_3_Agent_System/Agent_Engineer_Three_Mental_Models/Agent_Engineer_Core_Stacks/Enterprise_Agentic_AI_6_Ideas）。新增 3 个 idea_museum seeds。笔记总数：96。剩余未处理 raw 文件：91 个（待下次批量处理）。
- 2026-05-21（第十八次编译）：编译 1 个新 raw 文件（Agentic AI 企业级落地方案）。新增 1 页（Enterprise_Agent_Playbook）。笔记总数：86。
- 2026-05-20（第十五次编译）：编译 7 个新 raw 文件。新增 1 页（AI_Native_Startup_Playbook）。更新 4 页。笔记总数：64。
- 2026-05-20（第十六次编译）：编译 22 个 raw/SAP/ 文件。新增 20 个页面（Part 7 SAP Agent Engineering 完整子库）。笔记总数：84。
- 2026-05-15（第十三次编译）：编译 33 个未处理 raw 文件。新增 1 页（Hermes_Agent）。更新 8 个现有页面。跳过：23 个重复/范围外文件。笔记总数：63。
- 早期（第一至十二次）：建立基础知识体系。详见 _history/decisions.md。
