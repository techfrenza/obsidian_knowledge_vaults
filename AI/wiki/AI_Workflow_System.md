---
title: AI Workflow System
aliases: ["AI Workflows", "业务流程自动化", "Workflow-First", "5阶段实施"]
tags: [workflow, automation, orchestration, email-ops, content-engine, ai-first]
category: enterprise-ai
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# AI Workflow System

Parent: [[index]]

> 核心论点：**Workflow-First**（先定义业务流程，再用 AI 连接），而非 Tool-First。目标是让 AI 处理可重复的 🟢/🟡 部分（通常 60-70%），人类专注 strategy 和 growth。

---

## 核心分类框架

每个业务流程标记三类：

| 标记 | 含义 |
|------|------|
| 🟢 | 全自动（AI 直接执行） |
| 🟡 | AI 辅助 + 人工审核 |
| 🔴 | 必须人工处理 |

**目标**：大多数业务 60-70% 属于 🟢/🟡，识别并自动化这部分。

---

## 5 阶段实施路径（4-8 周见效）

### Phase 1（Week 1）：Map Your Workflows
用 Notion/Google Docs 记录所有 recurring workflows（销售、营销、支持、运营、财务）。  
每条记录：触发器、步骤、工具、输出、耗时、🟢🟡🔴 标记。

### Phase 2（Week 2）：Design Architecture
每个 Workflow 的 5 个组件：

```
Trigger           → 邮件/定时/Webhook
Input Processing  → 解析提取
AI Processing     → Claude + Prompt + Tools
Output Routing    → 推 CRM/Drive/Slack
Quality Check     → 规则验证 + 人工审核队列
```

先在 Miro/纸上画框图，不急于实现。

### Phase 3（Weeks 3-6）：优先建 3 个高 ROI Workflow

#### 1. Email Operations Center（最优先）
```
新邮件 → Claude 分类
  ├── 销售询盘 → 提取 + 评分 + 草稿 + CRM 更新
  ├── 客户支持 → 搜知识库 + 生成回复
  ├── 发票     → 提取金额 + 更新 tracker
  └── 内部邮件 → 总结行动项
```
**⚠️ 客户内容必须人工审核后才能发送。**

#### 2. Report Factory
```
定时拉 CRM/Analytics/财务数据
→ Claude 生成报告（指标/趋势/异常）
→ 保存 + 分发 + 一致性验证
```

#### 3. Content Engine
```
周一生成 10 个 idea
→ 人工挑选 3 个
→ Claude 写草稿 + 多平台改写
→ 人工审核（15-20 分钟，原需 4-6 小时）
```

### Phase 4：连接器集成
用 **n8n / Zapier / Make** 连接 Workflow——松耦合架构，避免连锁失败。（云端原生方案参见 [[Claude_Code_Routines]]）

### Phase 5：监控与进化
- **Success Rate**：目标 95%+
- **Time Saved**：每周统计
- **Quality Score**：输出质量评分
- 每月审视 + 季度跟进新 AI 能力

---

## Claude 记忆强化三层系统

配合 Workflow 系统使用，解决 Claude 默认"失忆"问题：

### Layer 1（5-10 分钟，90% 用户够用）
- Settings → Memory：清理垃圾，手动加核心偏好/角色
- Projects：每个 workflow 建专用 Project，上传 PDF Instructions
- 对话中直接说"记住：我喜欢 bullet points 输出"

### Layer 2（~60 分钟，中级用户）
桌面建"Claude Master Folder"，含 4 个 `.md` 文件：

```
Instructions.md  → 规则 + "Update Memory.md with preferences"
Memory.md        → 活脑：Preferences/Corrections/Patterns
Context.md       → 项目上下文
archives/        → 每周备份
```

在 Claude Desktop/Cowork 中 attach 整个文件夹，自动读写，跨聊天持久记忆。

### Layer 3（1-2 小时，高级第二大脑）
- **Notion 版**：Settings → Connectors 启用 Notion，建 Memory Database
- **Obsidian 版（最强）**：Obsidian Vault + Claude Desktop 指向 → 注入 Karpathy LLM Knowledge Base prompt → 扔笔记/CSV/文章，Claude 自动构建 evolving wiki（参见 [[Cross_Platform_Memory]]）

---

## Claude 产品地图（任务快速匹配）

| 任务类型 | 入口 |
|---------|------|
| 写作/研究/项目管理 | Claude App + Projects + Artifacts |
| 视觉设计/原型/演示 | Claude Design（画布实时生成） |
| 代码/文件/自动化 | [[Agent_Harness_Engineering|Claude Code]] |
| 构建产品/集成 | Claude API（[[Managed_Agent_Memory|Memory]] + [[Claude_Code_Skills|Skills]] 持久服务） |
| 业务流程自动化 | [[Claude_Cowork|Claude Cowork]] |

---

## 模型选择规则（1 秒决策）

| 复杂度 | 模型 |
|--------|------|
| 日常工作 | Sonnet（默认） |
| 简单快速 | Haiku |
| 复杂推理/高风险决策 | [[Opus_4_7_Migration|Opus]] |

---

## 7 天上手路径

| 天 | 行动 |
|----|------|
| Day 1 | 建第一个 Project，导入文件，写 Instructions |
| Day 2 | 所有输出用 Artifacts 保存，形成可复用资产 |
| Day 3 | Claude Design 做简单原型或 PPT |
| Day 4 | 打开 Claude Code，读一个真实项目文件夹 |
| Day 5 | 写 CLAUDE.md，让它按文档执行一个小修改 |
| Day 6 | 解决实际问题（先要 Plan，再执行） |
| Day 7 | 打包一个 Skill 或 Workflow，存起来重复用 |

---

## 通用 Prompt 结构模板

```
Goal：最终要达成什么
Context：提供全部背景和文件
Constraints：明确不能做什么
Definition of done：成功标准
Verification：要求它自检或跑测试
```

复杂任务额外加：**"先输出 Plan，我确认后再执行。"**

---


## 程序员 AI 自动化开发工作流（4 技能链）

**核心原则**：把 AI 从"自由写代码"变成"按文档跑流水线"。整条链靠**前期把关**——需求文档和技术文档两道关把住，后面走歪的概率趋近零。

[Source: raw/程序员从零搭建 AI 自动化开发工作流.md]

### 准备阶段：让 AI 读懂你的项目

新项目上手第一步不是写代码，而是让 Claude 探索目录结构，生成 `ARCHITECTURE.md`。老项目尤其关键——没有架构文档，AI 会凭空脑补项目结构。

规矩显性化：把所有隐性约定（命名/缩进/注释/数据库建表规范/模块边界）全部写入规范文件。遇到一次错就补一条，规矩越细，AI 走偏概率越低。

### 四个核心技能

| 编号 | 技能 | 输入 → 输出 | 必须人工确认 |
|------|------|------------|------------|
| ① | **需求 Skill** | 策划原始需求 → 详细需求文档（含边界条件/异常/数据流向）| ✅ 是 |
| ② | **技术文档 Skill** | 需求文档 → 技术方案（模块/接口/数据结构/调用链路/兼容性风险）| ✅ 是 |
| ③ | **代码生成 Skill** | 技术文档 → 多 Agent 分工代码（任务拆分 + harness 流水线）| 否（自动执行）|
| ④ | **Bug 解决 Skill** | 完整报错日志+复现步骤 → 修复 patch+影响范围+回归清单 | 否（流程化）|

**"前期磨刀两小时，后面砍柴十分钟"**：整个工作流最值钱的时间花在①②确认上。

### 代码生成阶段细节（③）

使用 [[Agent_Harness_Engineering|harness]] + 多 Agent 架构：
1. 技术文档拆分为多个独立小任务（一个任务一个目标，不混）
2. 多 Agent 分工协作（业务逻辑 / 单测 / 配置各自独立）
3. 每个 Agent 强制先读架构文档 + 规范文档 + 技术文档
4. [[Claude_Code_Hooks|Hooks]] 确保每完成一个任务自动更新相关文档和索引

### Bug 解决 Skill 强制流程

防止 AI "满屏乱改"，按固定套路走：
1. 列出嫌疑文件和函数（不改无关代码）
2. 给出假设：bug 在哪行、为什么触发
3. 最小改动验证假设
4. 假设成立后提修复 patch
5. 列出影响范围 + 回归检查清单

### 双 Review + 测试

- **第一轮**（逻辑 Review）：实现思路、边界条件、技术文档符合性
- **第二轮**（语法/编译 Review）：通过编译、lint、运行时 bug
- 两轮可用不同 Agent 互相挑刺（同一 Agent 自审等于没审）
- 测试方案：BDD（业务逻辑复杂）/ GoogleTest / Catch2（C++）——先补核心路径测试，慢慢扩展覆盖率

### 持续迭代原则

每次翻车补一条规则：规则不够细 → 补规范文件；需求 Skill 漏字段 → 补模板；Review 总漏同类问题 → 加专项检查项。跑得越久，工作流越贴合项目。

**与 [[CLAUDE_md_Best_Practices]] 的关系**：CLAUDE.md 管理全局规则，本工作流的四技能链是 CLAUDE.md 规则的执行层实例化。

---

## 关联实体（更新）

- [[AI_Orchestration_System]] — AI Workflow System 的技术实现层（Plan-First + Night Queue）
- [[Claude_Cowork]] — Workflow 系统在非开发者场景的专属平台
- [[Agent_Harness_Engineering]] — 开发者侧的 Harness 工程对应 Workflow System
- [[Cross_Platform_Memory]] — Layer 2/3 记忆系统的 Obsidian 实现
- [[CLAUDE_md_Best_Practices]] — Instructions.md / CLAUDE.md 的写法规范
- [[Claude_Code_Skills]] — 四技能链中每个 Skill 的工程实现规范
- [[Human_In_The_Loop]] — 需求确认步骤的 HITL 设计模式
- [[Multi_Agent_Architecture]] — 代码生成阶段多 Agent 协作的架构原则

*[Source: raw/Claude AI knowledge.md, raw/Claude系统化使用.md, raw/程序员从零搭建 AI 自动化开发工作流.md]*
