---
title: GBrain Architecture（个人 AI 第二大脑）
aliases: ["GBrain", "Fat Skills + Thin Harness", "Garry Tan GBrain", "Skillify"]
tags: [gbrain, fat-skills, thin-harness, knowledge-graph, skillify, entity-propagation]
category: enterprise-ai
parent: "[[Harness_Engineering_Deep_Dive]]"
created: 2026-05-10
date: "2026-05-10"
---

# GBrain Architecture（个人 AI 第二大脑）

Parent: [[Harness_Engineering_Deep_Dive]]

> 核心论点：**模型是引擎，Skill + Brain 才是车。** Fat Skills + Thin Harness + 持续 Skillify = 个人 AI 从玩具到 compounding nervous system。来源：Garry Tan 的 GStack 开源架构（github.com/garrytan/gstack）。

---

## 四层核心架构

| 层 | 组件 | 说明 |
|---|------|------|
| 路由层 | Thin Harness（OpenClaw / Hermes Agent / Claude Code Router） | 几千行代码，只负责"消息 → 匹配 Skill → 执行" |
| 技能层 | Fat Skills（100+ 独立 Markdown 技能文件） | 每个只做一件事，存于 Git 仓库 |
| 知识层 | Fat Data（100,000 页结构化知识库） | 每人/每公司/每会议/每本书一页 |
| 推理层 | 模型无关路由 | Skill 内部决定调用哪个模型（Opus 4.7/GPT-5.5/DeepSeek） |

---

## 目录结构模板

```
brain-repo/
├── skills/          # 所有 SKILL.md（可 Git PR 协作）
├── people/          # 每人一页
├── companies/       # 每公司一页
├── meetings/        # 每会议一页 + entity propagation
├── books/           # 每本书的 mirror 输出
├── crons/           # 100+ 个每日定时任务
└── resolver.md      # 路由表（Skill 触发条件）
```

---

## 三大杀手级技能

### 1. Skillify 元技能（最强递归工具）

触发：手动完成一次重复工作后立即运行 `skillify this workflow`

系统自动：
1. 分析刚刚发生的全流程
2. 提取可重复 pattern
3. 自动生成带 trigger、edge cases 的 SKILL.md
4. 注册到 resolver

**价值**：将"重复劳动"转化为"可复用 Skill"，形成自我进化飞轮。

### 2. Book-Mirror Skill（知识深度映射）

触发：`book mirror [书名]`

```
Steps:
1. 提取全书所有章节
2. 对每章运行 sub-agent：左=作者原意总结 / 右=映射到个人 brain
3. 跨模型互评（Opus + GPT-5.5 + DeepSeek 三模型 cross-modal eval）
4. 输出 30k 字双栏 Markdown + PDF
5. 自动更新所有相关 person/company 页面（entity propagation）
```

### 3. Meeting Workflow（会议前后一体化）

**Meeting-Prep（2 分钟自动生成）**：
- 拉取目标人物完整 brain page（timeline + current state + open threads）
- 合并最新传记、播客、文章
- 生成：3 个 conversation hooks + 3 个 demo scripts + worldview overlap/divergence
- 输出 `meeting-prep-[name].md`

**Meeting-Ingestion（会后自动运行）**：
- 读 transcript → structured summary
- Entity propagation：遍历所有提到的人/公司，更新他们的 brain page
- 标记 new insights、action items、follow-up

---

## 知识页面 Schema（每页必备结构）

每一个 person/company/meeting/book 页面固定包含：

1. **Compiled Truth**（顶部）：当前最佳理解，持续更新
2. **Append-only Timeline**（按时间倒序）：所有事件记录
3. **Raw Data Sidecar**：原始来源、transcript、PDF

---

## Entity Propagation（实体传播机制）

**定义**：当某个事件影响多个实体（人/公司/概念）时，自动更新所有相关页面。

场景：会议中提到了 A 公司的 B，则：
- B 的 person page → 更新 latest interactions
- A 的 company page → 更新 relationship status
- 本次 meeting page → 标记已处理

这是知识库保持"鲜活"的核心机制，避免数据孤岛。

---

## 与现有体系的融合（Claude Code + MCP + Multi-Agent）

```
GBrain = Obsidian 第二大脑的"增强版长期记忆层"
         ↓
所有 7-Agent Orchestrator 输出 → media-ingest → entity propagation
         ↓
Skill 库直接复用已有的 SKILL.md + CLAUDE.md
         ↓
MCP Connectors 负责实时拉 Gmail/Calendar/Slack 喂进 brain
```

最终目标：从"Claude 聊天" → "24/7 神经系统"（100k 页 + 100+ skills + 15 crons）

---

## 启动最小 SOP（7 天见效）

1. 安装 OpenClaw / Hermes Agent（或直接用 Claude Code + 文件系统）
2. Fork `github.com/garrytan/gbrain`（39 个 installable skills 已内置）
3. 先建一个 Book-Mirror：扔一本正在读的书，跑一次 skillify
4. 每天加 3 个 cron（email-triage、calendar-check、media-ingest）
5. 每周运行一次 cross-modal eval，修复 Skill 中的 factual error

---

## ECC（Everything Claude Code）生态补充

ECC 是 GBrain 思想在 Claude Code 生态中的具体实现（Anthropic Hackathon 获奖项目，153k+ stars）：

- **38 专业化 Agent**：planner（任务拆解 + 分配）/ security-reviewer / code-reviewer / debugger
- **156 Skills**（按需加载，不常驻 context）
- **AgentShield 安全扫描**：1282 个测试，扫 CLAUDE.md 硬编码 secret、settings.json 权限、MCP server CVE、Hooks 注入
  - `--opus` 模式：红队（攻击者 agent）+ 蓝队（防御者 agent）+ 审计员 agent 三方互评
- **持续学习系统**：跨 session 观察用户编码模式，confidence 分数从 0.3 → 0.9，自动应用编码风格

安全背景（2026-01）：某 marketplace 12% 的 skills 是恶意软件，CVSS 8.8 CVE 导致 17,500 实例可被 RCE。

---

## Skillify 10步完整流程（生产级 Skill 标准）

来源：Garry Tan "skillify manifesto"。每次 Agent 失败后执行此流程将错误转化为永久结构修复：

```
□ 1. SKILL.md — 合同（name, triggers, rules）
□ 2. 确定性代码 — scripts/*.mjs（代码能做的不让 LLM 做）
□ 3. 单元测试 — vitest
□ 4. 集成测试 — 真实端点验证
□ 5. LLM evals — 质量 + 正确性（LLM-as-judge）
□ 6. Resolver trigger — 写入 AGENTS.md 路由表
□ 7. Resolver eval — 验证 trigger 确实路由到正确 Skill
□ 8. check-resolvable + DRY audit — 消除暗 Skill + 重复 Skill
□ 9. E2E smoke test — 全链路端到端测试
□ 10. Brain filing rules — 知识归档路径
```

未通过全部 10 步 = 不是 Skill，只是"恰好能运行的代码"。

**关键洞见**：Latent（LLM判断）vs Deterministic（确定性代码）的空间分配是 Skill 设计的核心——日历查询、时间计算等确定性任务绝不在 LLM 推理空间执行。

---

## Hermes Agent Auto-think / Auto-build 循环

来源：@gkisokay 的 Hermes Agent 架构

**核心分工**：
- **Auto-think**：决定**什么值得构建**（idea intake层）
- **Auto-build**：决定**什么能构建**，验证并留下收据

### 完整执行链（13步）

```
1. Research → 收集证据（结构化evidence）
2. Dreamer → 注意信号，塑造候选 idea contract
3. Intent Review → 早期过滤（idea是否ready）
4. Main Review → 审批门禁（Dreamer不能审批自己的作品）
5. Main → 编写 Product Plan（allowed paths, planned files, non-goals, verification commands）
6. Coder → 将 Product Plan 转化为 Build Plan
7. Coder → 在 allowed paths 内实现（不能静默扩展scope）
8. Coder → 记录 verification
9. QA → 独立验证（不信任 Coder 摘要）
10. Delta → 对比 Coder 和 QA 证据（confirmed/drift/regression/missing_evidence）
11. Trust Report → 检查整个 room health（clean/watch/investigate）
12. Retention → 决定 artifact 命运（keep/improve/park/prune）
13. Operator → 查看 Control Room
```

**关键护栏**：Dreamer 不能审批自己；Coder 不能静默扩展 scope；QA 不能橡皮图章；Retention 不能静默删除

---

## Meta-Meta-Prompting：Skills That Build Skills

来源：Garry Tan《Meta-Meta-Prompting》

**递归核心**：Skillify 是一个**元技能**，它能创造其他 Skills：
- Skills 相互组合：book-mirror 调用 brain-ops + enrich + cross-modal-eval + pdf-generation
- 改进一个 Skill → 所有使用它的工作流自动变强
- 积累 100k 页知识库 → 每本书 mirror 越读越深（第20本书知道前19本的内容）

**100+ crons 的含义**：不是调度任务，而是持续运行的"神经系统"——每次会议/邮件/文章都自动更新 brain。

---

## 矛盾与争议

Cross-modal eval（多模型互评）成本高 vs 单模型快速迭代。GBrain 建议每周一次批量评估，而非每次推理都做。

---

## 关联概念
- [[Harness_Engineering_Deep_Dive]] — Thin Harness 定义：最小路由层 + Fat Skills 可替换知识层
- [[Contextmaxxing]] — GBrain 是 Contextmaxxing 的个人级实现：预编译知识 > 查询时重建
- [[Claude_Code_Skills]] — Skills 的底层设计规范（description 写法、负触发优先原则）
- [[Skill_Design_Patterns]] — GBrain Skills 的设计模式选择（Book-Mirror = Pipeline + Generator 组合）
- [[Agentic_Memory_System]] — GBrain 的 Compiled Truth + Append-only Timeline = 外部记忆 + Episodic 记忆
- [[Multi_Agent_Architecture]] — GBrain orchestrator 输出与三层 Skills/Agents/MCP 架构的融合点
- [[Prompt_Engineering_Advanced]] — Metaprompting = GBrain Skillify 的理论基础
- [[Agent_Engineer_MOC]] — Agent Engineer 完整学习地图

*[Source: raw/GBrain fat skills+Thin Harness.md, raw/Anthropic hackathon winner automated his entire Workflow. Free repo replaces a $15KMonth Dev Team.md, raw/How to really stop your agents from making the same mistakes.md, raw/How to Build a Hermes Agent That Finds Important Work and Builds It Autonomously.md, raw/Meta-Meta-Prompting The Secret to Making AI Agents Work.md]*

- [[GBrain_Fat_Thin_Architecture]] — GBrain 架构详细实现：Meeting Skill / Book-Mirror / 7天启动SOP
