---
title: Claude Code Skills
aliases: ["Skill 封装模式", "SKILL.md", "Claude Skills", "Karpathy Loop"]
tags: [skills, claude-code, automation, karpathy-loop, cowork]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Code Skills

Parent: [[index]]

> 核心论点：Skill 是将重复工作流打包为可自动触发指令的机制。触发准确率的关键在于 **description 字段的写法**，而非 Skill 本体内容。

---

## Skill 文件位置

- 全局个人：`~/.claude/skills/skill-name/SKILL.md`
- 项目级：`.claude/skills/`（git 提交，团队共享）

---

## 六大必要模式

### Pattern 1：Description 写触发时机
- 坏：`description: "写测试"`
- 好：`description: "当用户要求生成测试用例、修复 linter 错误或验证新功能时触发，关键词：test、vitest、unit test"`
- **规则**：description <50 字符的 Skill 触发次数少 3-5 倍；前 250 字符决定是否加载

### Pattern 2：指令式而非对话式
- 使用 imperative verbs：`MUST`、`DO`、`OUTPUT`
- 避免："你能帮我写测试吗？"
- 改为："1. 先读当前文件 2. 分析边缘 case 3. 生成测试代码"

### Pattern 3：明确输出格式
```
输出必须是：
- 第一行：测试文件路径
- 后面用代码块
- 最后加"测试通过/失败"总结
```

### Pattern 4：必须包含 Read First 步骤
```
先运行 git status 和 read [当前文件]，再执行任务
```

### Pattern 5：明确定义 Out of Scope
```
本 Skill 不做：重构现有代码、添加新依赖、修改非测试文件
如果用户要求这些，停止并询问澄清
```

### Pattern 6：总长度 <500 行
- 超过 2000 行会吃 5000+ tokens，Claude 容易忽略后面指令
- 超长时拆分：主 SKILL.md 里 `include: helper-rules.md`

---

## 完整模板（/commit Skill）

```yaml
---
name: commit
description: 当用户说 commit、生成 commit message 或需要 git 提交时触发，关键词：commit、push、PR
---

1. 运行 git status 和 git diff --cached
2. 分析改动
3. 输出格式：
   - 类型: [feat/fix/refactor]
   - 标题: 一行总结
   - 内容: bullet points
   - 影响: 任何 breaking change
4. 不做：实际执行 git commit（只生成 message）
```

---

## Karpathy Loop — 自动技能提升

每晚运行，第二天直接用 94% 准确版：

```
Use the Auto Research methodology to build a self-improving skill system
for my [skill name] skill.
The skill file is at [完整路径].
Eval criteria: [4-6 条 yes/no 标准].
Every 2 minutes, generate 10 outputs using the skill,
pass them through the eval suite, score pass rate,
and improve the skill prompt to increase the pass rate.
```

---

## 负触发优先原则

SKILL.md 里 80% 写"不做什么"，比正面触发更重要。  
调试命令：`"When would you use the [skill-name] skill?"` → Claude 逐字吐出描述，立即看出模糊点。

---

## Skills 完整机制（官方规范）

### 优先级层级
Enterprise → Personal → Project → Plugins（上级覆盖下级同名 Skill）

### 加载流程
1. Claude Code 启动时扫描 4 个位置，只加载 `name + description`
2. 用户请求语义匹配 → 弹出确认框 → 用户同意后才读入完整 SKILL.md 执行
3. 修改 SKILL.md 后需**重启 Claude Code** 才生效

### 与其他机制的边界

| 机制 | 触发方式 | 用途 |
|------|---------|------|
| CLAUDE.md | 每次对话自动加载 | 项目总则、统一规范 |
| Skills | 语义匹配时按需加载 | 任务级专业知识/流程 |
| Hooks | 事件驱动（保存/工具调用） | 自动副作用（linter/测试） |
| Subagents | 委托独立任务 | 上下文隔离执行 |
| MCP servers | 提供外部工具能力 | 数据库/API/服务接入 |

Skills 提供**本地知识与流程说明**，MCP 提供**外部系统工具能力**。常搭配：Skills 告知 Claude 何时/如何使用 MCP tools。

---

## Skill 文件夹结构（生产级标准）

来源：Perplexity 内部 Skill 设计指南 + @Av1dlive 实战总结

**Skill 不是一个文件，而是一个文件夹：**

```bash
your-skill/
├── SKILL.md        ← description（路由触发）+ 约束条件（不含已知规则）
├── scripts/        ← 确定性代码（git命令/linter/文件操作）
├── references/     ← 重型文档（API文档/style guide/错误表），按需加载
└── assets/         ← 模板/schema/输出格式
```

**三层 Context 成本**：

| 层 | 成本 | 内容 |
|----|------|------|
| Index tier | ~100 tokens，始终加载 | description 字段 |
| Load tier | 2000–8000 tokens，触发时加载 | SKILL.md 正文，目标 <1500 tokens |
| Runtime tier | 无限，仅按需读取 | references/ 里的一切 |

**最大化 references/ 的价值**：Style guide、API 文档、错误表 → 移入 references/。Skill 正文只说"consult references/api.md"，节省每次加载的 token 成本。

---

## Description 写法：路由触发器而非功能摘要

**核心规则**：description 是路由信号，不是 skill 说明书。

- **错误**：`"This skill helps with API debugging"`
- **正确**：`"Load when the user is getting a 4xx or 5xx from a service they own and are trying to diagnose it"`

**模板**：每条 description 必须以 `"Load when"` 开头，用用户实际使用的口语（`"my build is broken"` 而非 `"compilation error"`）

---

## Skill 正文写法：约束而非文档

**唯一测试**：*这句话若删掉，Agent 会做错吗？* → 否则删除。

- 模型已知的东西（git基础、Python语法、PR描述格式）不写
- 只写你的"品味"：`"never force-push to main"` / `"type stubs go in same file"`
- 写 Intent，不写步骤列表 → Intent 随模型升级自动变强，步骤列表会过时

---

## Gotcha 节：技能的制度记忆

追加模式（append-only）：每次 Agent 失败后**不重写 description**，只在 `## Gotchas` 节追加：

```markdown
## Gotchas
- If the repo uses pnpm workspaces, root package.json is NOT the entrypoint. Start from packages/.
- API rate limits reset at midnight UTC, not midnight local.
```

Gotcha 节是 Skill 的"经历记录"，越老越值钱。

---

## Eval-First 设计（先写测试，再写 Skill）

在写任何 Skill 之前，先写 10–20 个 queries：
- **Should-trigger**：10 条（定义 Skill 真正用于什么场景）
- **Should-NOT-trigger**：10 条（强制定义边界，防止与其他 Skill 碰撞）

无法写出测试 = 不需要这个 Skill。

---

## Karpathy autoresearch Loop — 自动技能提升

来源：Andrej Karpathy autoresearch 仓库（42k stars首周）

```
Use the Auto Research methodology from https://github.com/karpathy/autoresearch
to build a self-improving skill system for my [skill name] skill.
The skill file is at [完整路径].
Eval criteria: [4-6 条 yes/no 标准].
Every 2 minutes, generate 10 outputs using the skill,
pass them through the eval suite, score pass rate,
and improve the skill prompt to increase the pass rate.
```

实测结果：fundraising skill 从 70% → 94%；sales qualification 从 65% → 91%。

---

## 从教育内容到 Skill 的转换模式

**核心洞见**：Great prompts = education products copy-pasted into AI。  
为人类写的高清晰内容（课程模块、Checklist、操作手册）= 为 AI 写的最佳 prompt，无需重新"提示词工程"，直接复用。

**转换步骤（5 分钟）**：
1. 取已有的长形式教育内容（如"5000 字课程模块"）
2. 在顶部加一行 wrapper prompt：`You are my [角色]. Follow the exact process below:`
3. 粘贴原内容 → 完整 Skill Prompt 生成完毕

**为什么有效**：写给人类助理的指令 = 写给 Claude 的指令。Clarity of thought 是最高阶的 prompt engineering，不是特殊语法。

---

## 一步一测验证法（防止大型 Skill 崩溃）

**反模式**：一次性构建完整的多步骤 Skill → 第 3 步失败时难以定位根因，整体崩溃。

**正确流程**：
```
Step 1: 告诉 Claude 整个 Skill 的完整流程（用编号列表写清楚所有步骤）
Step 2: 只构建并测试 Step 1，输出合格后才继续
Step 3: 构建并测试 Step 2，以此类推
Step 4: 每一步用真实案例验证（输出问题 80% 来自"你的文档不清晰"，而非 Claude）
```

**持久化方案**：用 Notion/Obsidian 作为 Skill 知识库 + Claude Cowork 连接读取 → 链式调用多个步骤，比单纯 SKILL.md 更适合多步骤复杂 Skill。

*[Source: raw/How To Build A Claude Skill From Scratch (1-Hour Masterclass).md]*

---

## 生产级 Skill 10 大工程规则（2026 最新）

> 口诀：**描述负责路由，代码负责干活；断言卡死边界，裁判评估成果；路径严禁逾矩，大脑永不失忆。**

### 规则 1：SKILL.md = 路由契约（非使用说明）
- YAML Frontmatter 是给 Claude Code 核心层读取的路由总控
- 必须包含：`context: fork`（强制上下文隔离）、`background: true`（允许异步后台）
- `description` 用"渐进式披露"原则——首 250 字符即决定是否加载

### 规则 2：确定性代码处理固定逻辑
- **"确定性优于概率性"**：文件读取、类型检测、元数据提取用 `.mjs`/`.py` 脚本实现，100% 成功率
- 严禁让 LLM 用自然语言猜测如何操作文件系统

### 规则 3-5：三层测试金字塔
| 层 | 测试类型 | 目标 | 工具 |
|----|---------|------|------|
| 单元测试 | 针对纯函数断言（字符串处理、格式转换） | 0 概率出错 | Vitest / Node assert |
| 集成测试 | 真实文件 + 真实端点（读写物理文件，含 fixtures） | 链路可靠性 | test/integration.test.mjs |
| Eval（LLM 裁判） | 主观输出评估（Agent 分析质量、内容连接） | 通过/失败判定 | 轻量模型（Haiku）作裁判 |

**Eval 写法**：将明知会触发边缘情况的测试文本输入 Skill，由裁判模型判断输出是否满足标准（如"是否识别了知识冲突"）。无法写 Eval = Skill 边界不清晰。

### 规则 6-7：Resolver 路由测试
- 必须维护 10 条路由语境测试矩阵，验证触发词精确匹配（"read this" → brain-ingest，"帮我看看" → 通用对话）
- 触发失败时重新微调 description 中的"高频真实词汇"，而非修改 Skill 正文

### 规则 8：DRY 审核（跨 Skill 冲突检测）
- 每次引入新 Skill 前，交叉比对所有 `SKILL.md` 的 `writes_to` 权限路径
- 两个 Skill 同时拥有同一路径写权限 = 潜在脏数据风险，必须厘清职责边界
- 辅助工具：`claude doctor`

### 规则 9：E2E 烟雾测试
- 发布前必须跑完完整生命周期：捕获 → 提取 → 隔离 → 分析 → 双向链接 → 同步
- 检查 statusline 仪表盘：并发工具调用无死锁，Exit Code = 0

### 规则 10：刚性写入路径（归档规则硬编码）
- 在 `writes_to` 字段硬编码物理路径限制：
  - `people/`：仅限个人履历、时间线
  - `concepts/`：仅限可复用思维模型
  - `sources/`：原始未结构化数据
- Harness 在 Agent 试图跨路径写入时自动拦截

*[Source: raw/10 Rules to create Skills.md]*

---



- [[Claude_Code_Hooks]] — Skill 之外的确定性约束层（不适合放 Skill 的放 Hooks）
- [[Agent_Harness_Engineering]] — Skill 在 Harness 六层架构中的位置（Layer 3）
- [[CLAUDE_md_Best_Practices|CLAUDE.md Best Practices]] — Skill 的加载上下文来源
- [[Claude_Code_Subagents]] — 高输出任务的隔离执行层（与 Skill 互补）；`context: fork` 的底层机制
- [[Claude_Cowork]] — Cowork Plugin 是非开发者侧的 Skill 等价物（同一抽象，不同受众）
- [[Skill_Design_Patterns]] — 五大 SKILL.md 设计模式（Tool Wrapper/Generator/Reviewer/Inversion/Pipeline）
- [[Multi_Agent_Architecture]] — 三层架构中 Skills 层的单源权威原则与 sync 脚本模式
- [[Claude_Code_Advanced_Features]] — Skill 在完整 Claude Code 高级功能体系中的位置
- [[Skill_Ecosystem]] — Skills 生态资源地图；DRY 审核中 `claude doctor` 辅助检查的工具生态
- [[GBrain_Architecture]] — Fat Skills + Skillify 元技能 + 10步 Skillify 流程
- [[Prompt_Engineering_Advanced]] — Description 写法 = 应用级 Prompt Folding（分类路由）；Eval 裁判 = Metaprompting 循环的验证层
- [[SAP_Agent_Testing]] — 企业级测试五层金字塔（与 Skill 三层测试金字塔的关系：后者是前者在 Skill 粒度的专项实现）

*[Source: raw/Claude Code Skills.md, raw/AI Agent Tips.md, raw/Skills summary.md, raw/How to Use Claude Skills to Automate Any Workflow (Full Course).md, raw/The Ultimate Skill Playbook for Claude Code (Builder's Guide) 1.md, raw/Your Claude Code Skills Are Stuck. Here's How to Fix That While You Sleep.md, raw/10 Rules to create Skills.md, raw/Claude Code - Skill & Subagent advanced use.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图
- [[Skill_Engineering_10_Rules]] — Skill 工程十大规则：生产级 Skill 完整工程手册
- [[Claude code CLI -running 2 skills in background and front]] — CLI 并行技巧：后台技能（context:fork + background:true）、Worktree 隔离、/loop + /batch 批量并发