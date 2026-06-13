---
date: 2026-05-25
source_files:
  - "agent-harness-context-discipline-synthesis.md"
  - "ai-native-system-philosophy-synthesis.md"
  - "claude-code-control-philosophy-synthesis.md"
  - "claude-code-automation-os-synthesis.md"
  - "enterprise-gbrain-agent-architecture-synthesis.md"
  - "agent-engineer-mental-models-synthesis.md"
  - "ai-orchestration-os-synthesis.md"
source_notes:
  - "[[Agent_Harness_Engineering]]"
  - "[[Claude_Code_Skills]]"
  - "[[CLAUDE_md_Best_Practices]]"
  - "[[Context_Engineering]]"
  - "[[AI_Agent_247_Architecture]]"
  - "[[AI_Agent_Payments]]"
  - "[[AI_Native_Startup_Playbook]]"
  - "[[AI_OS_Framework]]"
  - "[[AI_Orchestration_Practice]]"
  - "[[Claude_Code_Security]]"
  - "[[Claude_Code_Self_Evolving]]"
  - "[[Claude_Code_Settings]]"
  - "[[Claude_Code_Subagents]]"
  - "[[Claude_Cowork]]"
  - "[[Claude_Code_Hacks]]"
  - "[[Claude_Code_Hooks]]"
  - "[[Claude_Code_Routines]]"
  - "[[Claude_Code_MOC]]"
  - "[[Claude_Code_Product_Positioning]]"
  - "[[Enterprise_Agent_Playbook]]"
  - "[[Enterprise_Agentic_AI_6_Ideas]]"
  - "[[GBrain_Architecture]]"
  - "[[GBrain_Fat_Thin_Architecture]]"
  - "[[Harness_Engineering_Advanced]]"
  - "[[Agent_Engineer_MOC]]"
  - "[[Agent_Engineer_Mental_Models]]"
  - "[[Agent_Engineer_Roadmap]]"
  - "[[Agent_Engineer_Three_Mental_Models]]"
  - "[[Agentic_Loop]]"
  - "[[AI_Orchestration_System]]"
  - "[[AI_Team_Coding_Practice]]"
  - "[[AI_Workflow_System]]"
  - "[[Agent_Context_Architecture]]"
  - "[[Agent_Engineer_Core_Stacks]]"
tags: [synthesis, consolidated, agent-engineering, harness, context, control-philosophy, enterprise-agent, mental-models, orchestration]
---

# Agent Harness 工程全景综合（整合 7 份合成笔记）

> 本文整合以下 7 份存在大量主题重叠的合成笔记，保留所有独特洞察，去除近义重复内容：
> 1. `agent-harness-context-discipline-synthesis.md`（核心笔记：[[Agent_Harness_Engineering]]、[[Claude_Code_Skills]]、[[CLAUDE_md_Best_Practices]]、[[Context_Engineering]]、[[log]]）
> 2. `ai-native-system-philosophy-synthesis.md`（核心笔记：[[AI_Agent_247_Architecture]]、[[AI_Agent_Payments]]、[[AI_Native_Startup_Playbook]]、[[AI_OS_Framework]]、[[AI_Orchestration_Practice]]）
> 3. `claude-code-control-philosophy-synthesis.md`（核心笔记：[[Claude_Code_Security]]、[[Claude_Code_Self_Evolving]]、[[Claude_Code_Settings]]、[[Claude_Code_Subagents]]、[[Claude_Cowork]]）
> 4. `claude-code-automation-os-synthesis.md`（核心笔记：[[Claude_Code_Hacks]]、[[Claude_Code_Hooks]]、[[Claude_Code_MOC]]、[[Claude_Code_Product_Positioning]]、[[Claude_Code_Routines]]）
> 5. `enterprise-gbrain-agent-architecture-synthesis.md`（核心笔记：[[Enterprise_Agent_Playbook]]、[[Enterprise_Agentic_AI_6_Ideas]]、[[GBrain_Architecture]]、[[GBrain_Fat_Thin_Architecture]]、[[Harness_Engineering_Advanced]]）
> 6. `agent-engineer-mental-models-synthesis.md`（核心笔记：[[Agent_Engineer_MOC]]、[[Agent_Engineer_Mental_Models]]、[[Agent_Engineer_Roadmap]]、[[Agent_Engineer_Three_Mental_Models]]、[[Agentic_Loop]]）
> 7. `ai-orchestration-os-synthesis.md`（核心笔记：[[AI_Orchestration_System]]、[[AI_Team_Coding_Practice]]、[[AI_Workflow_System]]、[[Agent_Context_Architecture]]、[[Agent_Engineer_Core_Stacks]]）

---

## 一致主线

### 主线 1：Harness 重于模型（全 7 份笔记共同确认）

这是唯一贯穿所有 7 份笔记、无一例外的核心论断：**AI Agent 系统的可靠性和性能，不取决于底层模型的智能程度，而取决于围绕模型构建的"制度环境"（Harness）**。

量化实证（来源：[[Agent_Engineer_Roadmap]]）：同一模型 Opus 4.5 在不同 Harness 下性能差距 78% vs 42%。

各笔记的具体表述形式不同，但指向相同：
- 工程层面：`CLAUDE.md` 规则将错误率从 41% 降至 11%，靠规则约束而非更强模型（[[CLAUDE_md_Best_Practices]]）
- 触发层面：Skill 触发准确率由 `description` 字段决定，而非 Skill 本体内容（[[Claude_Code_Skills]]）
- 架构层面：AI OS 的 Four Cs（Context/Connections/Capabilities/Cadence）和 247 架构的三大生存规则，均将价值沉淀在规则系统而非 Prompt 本身（[[AI_OS_Framework]]、[[AI_Agent_247_Architecture]]）
- 控制层面：`settings.json` 的 `deny` 规则在物理层阻断危险操作，Claude 无法绕过（[[Claude_Code_Security]]、[[Claude_Code_Settings]]）
- 规模层面：Fat Skills + Thin Harness 是唯一可扩展的落地模式——路由层保持最薄，知识/技能层尽量厚重（[[GBrain_Fat_Thin_Architecture]]、[[Harness_Engineering_Advanced]]）

**统一论断**：模型是"不稳定的工程部件"，Harness 才是真正的产品。工程师的核心杠杆是"制度设计"，而非调用更好的模型或框架。

---

### 主线 2：制度演化飞轮（来源：笔记 1、2、4、5）

每次 Agent 失败不是损失，而是为下一次运行积累确定性知识。跨笔记观察浮现同一个飞轮结构：

```
错误事件 → 规则更新（CLAUDE.md / DECISIONS.md / SKILL.md）→ 行为约束增强 → 错误率下降 → 新错误暴露 → 下一轮
```

这个飞轮在不同层次的实现：
- 个人层（[[Claude_Code_Self_Evolving]]）：Corrections → Rules 自动 promote 循环
- 知识库层（[[log]]）：Health Score 持续从 77 → 85 → 更高，每次 lint 运行后改善
- 企业层（[[Enterprise_Agent_Playbook]] Idea 5）：N8N 每日收集 Agent Session Logs → Claude 自动优化 Prompt + 更新 CLAUDE.md → Self-Reinforcing Loop
- GBrain 层（[[GBrain_Architecture]]）：Skillify 元技能——完成重复工作后立即自动生成 SKILL.md

**关键洞察**：Agent 系统的可靠性不是"设计出来的"，而是在生产运行中通过制度演化"长出来的"——这与传统软件工程的"设计-测试-发布"范式根本不同。

---

### 主线 3：最小接触原则（来源：笔记 3、4、6）

系统复杂度通过约束总量而非扩张总量来运作：
- Subagents 返回 ≤ 2000 token 摘要（[[Claude_Code_Subagents]]）
- CLAUDE.md 保持 < 80 行（[[CLAUDE_md_Best_Practices]]）
- settings.json 只开必要权限（[[Claude_Code_Settings]]）
- Cowork 的委托公式强调"避免清单"（[[Claude_Cowork]]）
- 单 Agent 优先，避免过早引入 multi-agent 复杂性（[[Agent_Harness_Engineering]]）

---

### 主线 4：AI 时代工程师角色转型（来源：笔记 2、7）

AI 系统的价值主张要求工程师角色的根本转变：从"写代码"转变为"设计和维护围绕 AI 运行的系统"。

具体体现：
- 人是架构师，AI 是可替换工程部件（[[AI_Native_Startup_Playbook]]、[[AI_Orchestration_Practice]]）
- 用编排系统（而非单次对话）驱动并行 Agents（[[AI_Orchestration_System]]）
- Workflow-First 思维：先定义业务流程，再用 AI 填充（[[AI_Workflow_System]]）
- 质量由系统基础设施保证，而非个人注意力保证（[[AI_Team_Coding_Practice]]）

---

### 主线 5：信任拓扑层次（来源：笔记 3 独有）

Claude Code 生态构建了一套递进的信任架构（单篇笔记无法显现，需跨视角才能看见）：

```
系统deny（零信任）→ CLAUDE.md规则（条件信任）→ Self-Evolving修炼（经验信任）→ Subagent Fork（继承信任）→ Cowork Meta-Prompt（委托信任）
```

每一层都在回答同一个问题：系统如何把"人类意图"稳定地转移给 AI 代理，同时防止意图在转移过程中失真。

---

### 主线 6：Claude Code 走向操作系统级基础设施（来源：笔记 4 独有）

KAIROS（24/7 主动修复测试）、Routines（云端无人值守批处理）、VPS 常驻会话三条线同时指向同一未来：**"AI 替代 DevOps 角色"而非"AI 辅助开发者"**。Claude Code 的真实产品演进方向是操作系统级自动化基础设施，这只有在审视全部自动化组件时才能看见。

---

### 主线 7：三大心智模型是统一的资源治理框架（来源：笔记 6 独有）

Agent 工程三大心智模型（Workflow vs Agent / 增强型 LLM / 上下文原语）并非并列教学条目，而是同一个**资源分配问题的三个切面**：
- "Workflow vs Agent"：决策权资源的归属（给代码还是给 LLM）
- "增强型 LLM"：能力资源的扩展方式（工具/检索/记忆三类增强）
- "上下文原语"：信息资源的治理机制（Write/Select/Compress/Isolate）

掌握标准：能在新系统设计中同时运用这三个维度做架构决策，而非仅能复述定义。

---

## 内在张力

### 张力 1：Context 压缩 vs 规则覆盖完整性（来源：笔记 1）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Context 应尽量压缩（Compress/Prune），防止 Context Rot，用最少高信号 Token 驱动行为 | [[Context_Engineering]] | CLAUDE.md 应包含足够规则，Karpathy 12 条规则在 65 行内才能保证遵守；过少规则导致错误率回升 | [[CLAUDE_md_Best_Practices]] |
| Skills 渐进式披露，仅在语义匹配时加载，节省 Token | [[Claude_Code_Skills]] | Contextmaxxing 策略"全喂 Context"（Boil the Ocean），将所有相关文件一次性注入以提升表现 | [[Contextmaxxing]] |

张力本质：**信号密度 vs 覆盖完整性**——压缩得越激进，遗漏关键约束的风险越高；堆叠越多规则，噪声和合规度下降的风险越高。

---

### 张力 2：单 Agent 简洁 vs 并行 Subagent 扩展（来源：笔记 1、3、4）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| 单 Agent 优先，避免过早引入 multi-agent 复杂性（七架构决策第一条）| [[Agent_Harness_Engineering]] | 并行 Subagents 可将独立任务分发给 3-10 个 session 同时执行，获得空间扩展收益 | [[Claude_Code_Subagents]] |
| Fork Subagent 继承父会话上下文，后续输入 Token 便宜 10 倍 | [[Claude_Code_Subagents]] | 并行 Subagent 每个保持独立上下文，主线程不受污染 | [[Claude_Code_Hacks]] |

---

### 张力 3：自主性 vs 制度约束（来源：笔记 2、3、6）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Agent Payments（x402/USDC/M2M）代表 Agent 完全自主化基础设施已就绪 | [[AI_Agent_Payments]] | Human-in-the-Loop 是生产系统的核心门禁，高风险操作（金额 > $500/删除/部署）必须物理拦截 | [[Human_In_The_Loop]] |
| 路径不确定时应升级为 Agent（提倡高自主性）| [[Agent_Engineer_Mental_Models]] | Agentic Loop 成本高、错误可累积，需要 HITL 与沙盒约束 | [[Agentic_Loop]] |
| Solo Founder Agent 目标：3 周内 70-80% 替代 3 名全职员工，Agent 自动打分直到达标 | [[Solo_Founder_Agent]] | AI-Native 创业手册：PMF 前应靠"英雄式推"维持，人类仍是最终价值判断者 | [[AI_Native_Startup_Playbook]] |

---

### 张力 4：自动化飞轮 vs 工程质量闸门（来源：笔记 5）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| Skillify 元技能：完成重复工作后立即自动生成 SKILL.md，形成"自我进化飞轮" | [[GBrain_Architecture]] | Skill 工程十大规则：生产级 Skill 需经过 10 步闭环（单元测试+集成测试+LLM Evals+Resolver 注册+E2E 烟雾测试），未通过全部 10 步 = 不是 Skill | [[Skill_Engineering_10_Rules]] |
| N8N 每日自动优化 Prompt + 更新 CLAUDE.md，形成 Self-Reinforcing Loop | [[Enterprise_Agent_Playbook]] | 规则需要人工审查精简，"每周审查并精简规则文件，删除过时规则" | [[Harness_Engineering_Advanced]] |

张力本质：**速度飞轮（自动化 Skillify + Self-Reinforcing Loop）与质量闸门（10步工程规则 + 人工审查）**之间的根本矛盾。GBrain 和 Enterprise Playbook 倾向于"先运转飞轮，快速迭代"；Harness Engineering 和 Skill_Engineering_10_Rules 倾向于"每个 Skill 都是生产合同，不能跳步"。

---

### 张力 5：速度优先 vs 质量闭环（来源：笔记 7）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| 5-10 个并行 Agents + auto-accept，最大化吞吐量 | [[AI_Orchestration_System]] | Plan→Work→Review→Compound 四步闭环，80% 价值在 Plan 和 Compound | [[AI_Team_Coding_Practice]] |
| Stack-First：先掌握 LangGraph + Claude SDK 底层原语，再从技术能力出发构建 Agent | [[Agent_Engineer_Core_Stacks]] | Workflow-First：先定义业务流程，再用 AI 连接；从业务痛点倒推工具选型 | [[AI_Workflow_System]] |
| Persistent Context 积累：持续建 CLAUDE.md + /examples 等上下文资产，越积越强 | [[AI_Orchestration_System]] | 轻量上下文管理：递归蒸馏后遗忘低频条目，每月 review + `forget` 保持系统轻量 | [[Agent_Context_Architecture]] |

---

### 张力 6：平台依赖 vs 架构自主（来源：笔记 2）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| 247 架构推荐托管平台依赖（如 Teamly），接受平台锁定换取运维减负 | [[AI_Agent_247_Architecture]] | Orchestration Practice 核心原则："所有 AI PR 都是你的 PR"，强调偏好无聊可测的原生 API，避免依赖 | [[AI_Orchestration_Practice]] |

---

### 张力 7：CLAUDE.md 的双重定位（来源：笔记 3）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| CLAUDE.md 只作建议，无法阻止读取，不能作为安全防线 | [[Claude_Code_Security]] | CLAUDE.md 是"职场说明书"，是上下文规则文件，是 AI 系统主要约束载体 | [[CLAUDE_md_Best_Practices]] |

---

### 张力 8：模型无关路由 vs 模型不稳定性约束（来源：笔记 5）

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| GBrain Fat Skills 强调模型无关路由：每个 Skill 内部自行决定调用 Opus 4.7、GPT-5.5 还是 DeepSeek | [[GBrain_Fat_Thin_Architecture]] | Harness Engineering Advanced 强调"将模型视为不稳定工程部件"，通过 Plan-First Workflow 和 Anti-Rot 约束来降低模型不确定性 | [[Harness_Engineering_Advanced]] |

---

## 涌现洞察

### 洞察 1：制度飞轮是所有层次系统护城河的统一本质

"制度飞轮"才是 AI-Native 系统真正的护城河，而非技术栈本身。这个规律横跨个人工具（Claude Code）、个人操作系统（GBrain/AI OS）、企业编排（Enterprise Agent Playbook）、经济层（Agent Payments）五个维度：无论规模如何，真正决定长期可靠性与竞争壁垒的都是规则沉淀 → 行为约束 → 错误暴露 → 规则更新的制度演化成熟度，而非某个具体工具或模型版本。

> 来源跨度：笔记 1（Karpathy Loop）、笔记 2（AI OS Cadence / 247架构）、笔记 4（Self-Evolving）、笔记 5（GBrain Skillify + Enterprise Self-Reinforcing Loop）

---

### 洞察 2：AI 编排操作系统是五个功能层的整体，缺一不可

AI 编排系统中，各组件并非独立工具，而是同一操作系统的五个功能层：
- **顶层调度器**：Orchestration System（并行任务分发）
- **业务需求输入层**：Workflow System（识别可自动化环节）
- **质量闭环层**：Team Coding Practice（Plan/Work/Review/Compound 四步）
- **持久化记忆层**：Context Architecture（四层记忆：Episodic/Semantic/Procedural/Working）
- **工程师能力基础层**：Core Stacks（LangGraph / Claude SDK 等底层原语）

缺任何一层，整个系统都退化为临时对话工具。速度与质量的矛盾解法不在于"折中"，而在于把质量检查编码进系统层（Hooks/CI/DECISIONS.md），使速度与质量不再对立。

> 来源：笔记 7

---

### 洞察 3："错误资本化"设计模式——Agent 失败即系统升级

GBrain Skillify（失败 → SKILL.md）、Enterprise Self-Reinforcing Loop（Session Logs → CLAUDE.md 更新）、Harness Engineering Compound 阶段（任务结束 → DECISIONS.md 更新）、Skill_Engineering_10_Rules 的 LLM-as-Judge Evals——四个表面来自不同语境的机制，实际上是同一个"错误资本化"设计模式在不同层次的实现：**Agent 每次失败不是损失，而是为下一次运行积累确定性知识**。

> 来源：笔记 5

---

### 洞察 4：三大心智模型是统一资源约束框架（见主线 7）

（已在主线 7 中完整描述，不重复）

---

### 洞察 5：控制层即信任层——信任拓扑递进（见主线 5）

（已在主线 5 中完整描述，不重复）

---

## 知识缺口

### 缺口 1：规则密度的收敛性——是否存在可量化最优点？

（来源：笔记 1）

**核心问题**：当规则累积到某个数量后，CLAUDE.md 的合规度曲线如何变化？是单调下降（过载），还是存在一个最优点后再下降？

已知上限警告（超 200 行合规度急剧下降、超 4000 tokens 降至 30%），但未回答：
1. 不同任务复杂度下，规则密度的最优解是不同的吗？
2. 能否用自动化评估（类似 Karpathy Loop 的 pass rate）持续追踪"规则集的边际收益"？
3. 跨团队共享 Harness 时，不同工程师添加的规则如何避免互相覆盖或语义矛盾？

**建议**：固定任务集，在 10/20/30/50 行规则下各测试 20 次，观察错误率曲线形态，结果更新至 [[CLAUDE_md_Best_Practices]] 的"合规上限"一节。

---

### 缺口 2：规则生命周期分配框架——规则在三层之间如何流动？

（来源：笔记 3）

**核心问题**：当 Self-Evolving 循环"毕业"了大量规则（规则增多），而 CLAUDE.md 又要保持 < 80 行（规则压缩），**哪些规则值得永久保留、哪些应降级为 `settings deny`、哪些应提升为 Subagent 专项职责**？

规则在三个层次（CLAUDE.md / settings.json / Subagent 角色定义）之间如何流动与退场，是一个空白。

**建议**：建立"规则分级矩阵"——根据规则的频率、风险等级、可自动化程度，决定它应落在哪一层，并设计对应的"降级/升级"触发条件。参考：[[Agent_Harness_Engineering]] 的 Scaling 三维度框架 + [[Claude_Code_Self_Evolving]] 的 promotion 条件。

---

### 缺口 3：Agentic 支付风险分类矩阵

（来源：笔记 2）

**核心问题**：当 Agent Payments（x402/USDC/M2M）与 HITL（人工门禁）共存时，"哪一类支付行为应触发 HITL 拦截，哪一类可以完全自主"的决策标准尚未被任何笔记明确定义。

[[AI_Agent_Payments]] 描述了技术可行性（200ms 结算），[[Human_In_The_Loop]] 描述了拦截机制（金额 > $500 触发审核），但两者之间缺少按**可逆性 × 金额量级 × 对手方可信度**三维度划定自主/半自主/强制人工审核边界的矩阵。

**建议**：参考 SAP Agent 写操作安全矩阵（NEVER fallback for writes）和 Multi_Agent_Architecture 的 Reader/Orchestrator/Resolver 分层，设计三层风险架构：只读发现层（无需审核）→ 小额微支付层（自动执行，事后审计）→ 高风险不可逆层（强制 HITL）。

---

### 缺口 4：Token 费用与长期质量漂移控制

（来源：笔记 4）

**核心问题**：在 10 个并行 Subagent + 全天运行 Routines + Fork 继承的自动化架构下，如何控制 Token 费用、防止长期质量漂移，并设定有效的回滚与熔断机制？

所有笔记均只点到问题（[[Claude_Code_Product_Positioning]] 提到"高额 Token 消耗"局限性），没有给出系统性解答。

**建议**：建立 `Claude_Code_Cost_Governance` 笔记，聚焦三个方向：
1. Token 预算控制策略（Subagent 输出 ≤ 2000 token 已有雏形，需系统化）
2. 质量门控 checkpoint 设计（Self-Evolving correction 频率指标可作为基础）
3. Routine 失败熔断机制（目前 Routines 只提到"异常时 post #dev-alerts and stop"，缺乏自动恢复设计）

---

### 缺口 5：Harness 质量的可量化评估指标体系

（来源：笔记 6）

**核心问题**：在 Agentic Loop 的实际生产运行中，如何量化"Harness 质量"？

已知 Harness 重要性的实证（78% vs 42% 性能差距），但缺少系统性评估指标体系：
- 什么是 Harness 质量的可测量代理指标（proxy metrics）？
- Context rot 发生的早期信号是什么，能否在 trace 日志中自动检测？
- HITL 介入率与 Agent 自主性之间的最优平衡点如何通过数据确定？

**建议**：在 [[Production_Reliability_MOC]] 和 [[SAP_Agent_Evaluation]] 中检索 Harness 评估框架的相关内容；若无，从 [[Karpathy_Methodology]] 的"Karpathy Loop + 评分工具"模式出发，构建专门的 Harness 质量评估笔记。

---

### 缺口 6：Skills 所有权边界——个人 Brain 与企业共享库的治理

（来源：笔记 5）

**核心问题**：GBrain 的个人 Fat Skills 体系与企业级 Multi-Agent Orchestration 结合时，Skills 的"所有权边界"应该如何设计？

当 Skillify 元技能为个人生成的 SKILL.md 进入企业共享 Skill 库时，如何防止"个人偏见"污染团队标准？[[Claude_Code_Skills]] 和 [[Skill_Engineering_10_Rules]] 都强调 Skill 应该存于 Git 仓库，但没有回答多人协作时的 Skill 治理问题（谁有权合并？如何版本化？个人 Brain 的 Skills 能否 fork 为团队 Skills？）

**建议**：研究 Skill 的"所有权→发现→共享→治理"完整生命周期，参考 [[SAP_Agent_ORD_Registration]] 的 ORD 端点标准和 [[SAP_Agent_UMS_Registry]] 的 UMS 注册机制，作为企业级 Skill 注册表设计参考。

---

### 缺口 7：多 Agent 并行场景下的上下文并发写入冲突

（来源：笔记 7）

**核心问题**：当多个 Agent 并行运行（5-10 并行会话）且共享同一套上下文资产（AGENTS.md/DECISIONS.md/CLAUDE.md）时，并发写入冲突如何处理？哪个 Agent 的决策有优先权？Context 如何在并行 Agent 间保持一致性而不产生版本分叉？

**建议**：研究多 Agent 并行场景下的"上下文协调协议"——借鉴 [[Enterprise_AI_Architecture]] 的状态机架构，探索能否将 DECISIONS.md/AGENTS.md 作为"单一真理来源"（File-system-as-State），通过乐观锁或事件溯源机制管理并发写入，形成真正的多 Agent 协作基础设施。

---

*引用笔记路径（全量）：*
- `/wiki/Agent_Harness_Engineering.md`
- `/wiki/Claude_Code_Skills.md`
- `/wiki/CLAUDE_md_Best_Practices.md`
- `/wiki/Context_Engineering.md`
- `/wiki/AI_Agent_247_Architecture.md`
- `/wiki/AI_Agent_Payments.md`
- `/wiki/AI_Native_Startup_Playbook.md`
- `/wiki/AI_OS_Framework.md`
- `/wiki/AI_Orchestration_Practice.md`
- `/wiki/Claude_Code_Security.md`
- `/wiki/Claude_Code_Self_Evolving.md`
- `/wiki/Claude_Code_Settings.md`
- `/wiki/Claude_Code_Subagents.md`
- `/wiki/Claude_Cowork.md`
- `/wiki/Claude_Code_Hacks.md`
- `/wiki/Claude_Code_Hooks.md`
- `/wiki/Claude_Code_Routines.md`
- `/wiki/Claude_Code_MOC.md`
- `/wiki/Claude_Code_Product_Positioning.md`
- `/wiki/Enterprise_Agent_Playbook.md`
- `/wiki/Enterprise_Agentic_AI_6_Ideas.md`
- `/wiki/GBrain_Architecture.md`
- `/wiki/GBrain_Fat_Thin_Architecture.md`
- `/wiki/Harness_Engineering_Advanced.md`
- `/wiki/Agent_Engineer_MOC.md`
- `/wiki/Agent_Engineer_Mental_Models.md`
- `/wiki/Agent_Engineer_Roadmap.md`
- `/wiki/Agent_Engineer_Three_Mental_Models.md`
- `/wiki/Agentic_Loop.md`
- `/wiki/AI_Orchestration_System.md`
- `/wiki/AI_Team_Coding_Practice.md`
- `/wiki/AI_Workflow_System.md`
- `/wiki/Agent_Context_Architecture.md`
- `/wiki/Agent_Engineer_Core_Stacks.md`
- `/wiki/Human_In_The_Loop.md`
- `/wiki/Multi_Agent_Architecture.md`
- `/wiki/Solo_Founder_Agent.md`
- `/wiki/Skill_Engineering_10_Rules.md`
- `/wiki/Skill_Design_Patterns.md`
- `/wiki/Karpathy_Methodology.md`
- `/wiki/Production_Reliability_MOC.md`
- `/wiki/LangGraph_Deep_Agents.md`
- `/wiki/Enterprise_AI_Architecture.md`
