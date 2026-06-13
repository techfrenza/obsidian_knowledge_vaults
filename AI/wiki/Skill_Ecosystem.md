---
title: Skill Ecosystem（技能生态与资源地图）
aliases: ["Skill Marketplace", "Skill Resources", "ECC Skills", "AI Tool Ecosystem"]
tags: [skills, marketplace, ecosystem, resources, agent-frameworks, mcp]
category: claude-tooling
parent: "[[Claude_Code_Skills]]"
created: 2026-05-10
date: "2026-05-10"
---

# Skill Ecosystem（技能生态与资源地图）

Parent: [[Claude_Code_Skills]]

> 核心论点：Skills 生态已形成多层分发体系（官方 → 社区 → 企业），选择标准：stars 数量/更新频率/description 质量。必须搭配 3 大 MCP 服务器才能让 Skills 有"手"。

---

## Skills 安装路径

```bash
# 个人（跨项目复用）
~/.claude/skills/

# 项目级（随代码进版本库，团队共享）
.claude/skills/
```

克隆 → 复制 → 重启 Claude Code。完成。

---

## 官方 Skills 资源（Anthropic）

| # | Skill | 用途 | 来源 |
|---|-------|------|------|
| 01 | PDF Processing | 读取、提取表格、填表、合并/拆分 | github.com/anthropics/skills/pdf |
| 02 | DOCX | 创建和编辑带修订的 Word 文档 | github.com/anthropics/skills/docx |
| 03 | PPTX | 从自然语言生成幻灯片 | github.com/anthropics/skills/pptx |
| 04 | XLSX | 公式、分析、图表（自然语言驱动） | github.com/anthropics/skills/xlsx |
| 05 | Doc Co-Authoring | 真正的人机协作写作 | github.com/anthropics/skills/doc-coauthoring |
| 06 | Frontend Design | 避免 AI 风格陷阱，真实设计系统 | github.com/anthropics/skills/frontend-design |
| 15 | Skill Creator | 元技能：描述工作流 → 5 分钟生成 SKILL.md | github.com/anthropics/skills/skill-creator |
| 19 | Brand Guidelines | 将品牌规范编码为 Skill，自动应用 | github.com/anthropics/skills/brand-guidelines |

---

## 社区精选 Skills

### 开发工程类

| Skill | 说明 | Stars |
|-------|------|-------|
| Superpowers (obra) | 20+ 实战 Skills，TDD/调试/计划执行管道 | 96k+ |
| Systematic Debugging | 根因分析优先，修复其次，4 阶段方法论 | S9.2 评分 |
| Karpathy Skills | Andrej Karpathy 个人工作流 Skills | - |
| Claude Inspector | 查看隐藏的 Claude Code 提示机制 | - |
| TDD Guard | 对 Agent 强制 test-first | - |

### 商业与创业类

| Skill | 说明 |
|-------|------|
| slavingia/skills | Sahil Lavingia（Gumroad CEO）个人工作流 |
| easychen/opc-methodology | One Person Company 方法论 |
| Marketing Skills by Corey Haines | 20+ Skills：CRO/文案/SEO/邮件序列/增长 |
| Claude SEO | 全站 SEO 审计，12 个子技能 |

### 知识与学习类

| Skill | 说明 |
|-------|------|
| Obsidian Skills (kepano) | Obsidian CEO 出品：自动标签/自动链接/vault-native |
| NotebookLM Integration | Claude + NotebookLM 桥接：摘要/思维导图/闪卡 |
| Context Optimization | 减少 Token 成本，KV-cache 技巧 |

---

## 必备 MCP 服务器（让 Skills 有"手"）

| 服务器 | 用途 | 特点 |
|--------|------|------|
| Tavily | 专为 AI Agent 设计的搜索引擎 | 4 工具：search/extract/crawl/map；1 分钟接入 |
| Context7 | 实时注入最新库文档到 LLM context | 防止 API 幻觉；只需在 prompt 加 "use context7" |
| Task Master AI | AI 项目管理器 | PRD → 结构化任务 + 依赖图 → 逐一执行 |

---

## Agent 框架生态

| 框架 | 定位 | Stars |
|------|------|-------|
| OpenClaw | 病毒式 AI Agent，持久化、多渠道、自写 Skills | 210k+ |
| LangGraph | Agent as Graph，多 Agent 编排 | 26.8k |
| CrewAI | 多 Agent + 角色/目标/背景故事 | - |
| Dify | 开源 LLM 应用构建器（Workflow + RAG + Agent） | - |
| pydantic-ai | 类型安全 Agent 框架 | - |

---

## 多 Agent 编排工具

| 工具 | 说明 |
|------|------|
| gstack (Garry Tan) | Claude Code 作为虚拟工程团队 |
| cmux | 并行运行多个 Claude Agent |
| claude-squad | 终端中并行 Agent 会话 |
| figaro | 在桌面编排 Claude Agent 舰队 |
| SWE-AF | 一个 API 调用启动整个工程团队 |

---

## 安全与基础设施

| 工具 | 说明 |
|------|------|
| AgentShield (ECC) | 1282 测试覆盖 Claude Code 配置安全（CLAUDE.md/settings/MCP/Hooks） |
| e2b/desktop | Agent 隔离虚拟桌面 |
| container-use (Dagger) | Agent 容器化运行环境 |
| agent-governance-toolkit (Microsoft) | Agent 安全中间件 |
| promptfoo | AI 模型自动安全测试 |

---

## Skills 发现平台

- **skillsmp.com** — 80k+ Skills，最大目录
- **skillhub.club** — 31k+ Skills，AI 评分
- **aitmpl.com/skills** — 模板库，一命令安装
- **awesome-claude-skills** — 精选 Skills 列表（22k+ stars）

---

## Skills vs 其他 Claude Code 机制（决策树）

```
重复说明的规范/约束  → CLAUDE.md（全局加载）
任务级专业知识/流程 → Skills（按需加载）
事件驱动副作用      → Hooks（确定性执行）
大块委托任务        → Subagents（上下文隔离）
外部系统接入        → MCP（工具能力层）
```

Skills 告诉 Claude **何时/如何用** MCP tools；MCP 给 Claude 提供**做什么的能力**。

---

## Skills 故障排查速查

| 症状 | 原因 | 解决方案 |
|------|------|---------|
| 不触发 | description 与请求语义不匹配 | 加入用户真实说法中的触发短语 |
| 不加载 | 目录结构错误 / 文件名错误 | SKILL.md 必须在子目录中，"SKILL"全大写 |
| 用错 Skill | 多个 description 太相似 | 让每个 description 更具区分度 |
| 被遮蔽 | Enterprise/高优先级同名 Skill 覆盖 | 改名（如 frontend-code-review）或与管理员协商 |
| 运行时失败 | 缺依赖/权限/路径 | chmod +x 脚本；统一用正斜杠 `/` |

优先级：Enterprise → Personal → Project → Plugins

---

## 矛盾与争议

Skill 数量 vs 可维护性：GBrain 追求 100+ Skills，但每个 Skill 都需要维护 description 精度和负触发规则。建议：先用 [[Skill_Design_Patterns]] 中的 5 种模式写少量高质量 Skills，再用 Skillify 元技能扩展。

---

## 关联概念
- [[Claude_Code_Skills]] — Skill 六大必要模式（底层设计规范）
- [[Skill_Design_Patterns]] — 五大 SKILL.md 设计模式（Tool Wrapper/Generator/Reviewer/Inversion/Pipeline）
- [[GBrain_Architecture]] — Fat Skills + Thin Harness 架构与 Skillify 元技能
- [[MCP_Integration_Playbook]] — MCP 工具实战清单（与 Skills 配合使用）
- [[Claude_Code_Hooks]] — Hooks = 事件驱动确定性层（Skills 的互补而非替代）
- [[Multi_Agent_Architecture]] — Subagents 共享 Skills 的 frontmatter 配置方式
- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图

*[Source: raw/Skills summary.md, raw/Anthropic hackathon winner automated his entire Workflow. Free repo replaces a $15KMonth Dev Team.md]*
