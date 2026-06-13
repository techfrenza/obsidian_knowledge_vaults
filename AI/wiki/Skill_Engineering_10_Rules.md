---
title: Skill 工程十大规则
parent: "[[Claude_Code_Skills]]"
tags: [skill-engineering, harness, production, testing, claude-code]
category: claude-tooling
stub: false
date: "2026-06-03"
---

# Skill 工程十大规则

生产级 Skill 不是写一段 Prompt，而是一套严密的**软件工程闭环**。

## 核心目录结构

```
~/.claude/skills/brain-ingest/
├── SKILL.md       # 契约、名称、触发器与 Frontmatter
├── ingest.mjs     # 确定性核心控制流脚本
├── brain-rules.json
└── test/
    ├── unit.test.mjs         # 纯函数单元测试
    ├── integration.test.mjs  # 真实数据集成测试
    └── eval.test.mjs         # LLM 裁判评估集
```

## 10 大规则

### 规则 1：SKILL.md YAML Frontmatter（契约）
路由总控，必须精炼，遵循"渐进式披露"原则。
- `context: fork` → 强制上下文隔离
- `background: true` → 允许异步挂起，不阻塞主 CLI

### 规则 2：确定性代码（.mjs 脚本）
**"确定性优于概率性。"** 固定逻辑必须用确定性 JS/ESM 脚本实现，禁止让 LLM 猜测文件解析逻辑。

### 规则 3：单元测试（纯函数断言）
字符串处理、格式转换等纯函数必须用 `Vitest` 或 Node `assert` 进行零概率出错断言。

### 规则 4：集成测试（真实数据端点）
用真实 fixture 文件测试整条读写链路，验证物理文件是否生成。

### 规则 5：LLM 评估（Evals / LLM-as-Judge）
主观输出（如分析块、冲突检测）无法用断言测，必须引入廉价模型（如 Haiku）作为裁判，输出 `<pass>/<fail>`。

### 规则 6：Resolver Trigger（路由表注册）
技能必须正式注册到 `~/.claude/config.json` 的 `resolvers` 节点，确保斜杠命令能精确唤醒。

### 规则 7：Resolver 评估（路由语境测试矩阵）
构建 10 个测试用例矩阵，确保高频真实词汇能 100% 正确路由，失败时微调 SKILL.md description。

### 规则 8：DRY 审核（无重复）
引入新技能前，交叉比对所有 `SKILL.md` 的 `writes_to` 权限路径，避免多 Agent 并发写入脏数据。

### 规则 9：E2E 烟雾测试（全流程）
模拟完整生命周期（捕获→提取→分析→链接→同步），检查无死锁、Exit Code = 0。

### 规则 10：大脑归档规则（硬性写入路径）
```
people/     → 个人履历、时间线、人类原创思想
concepts/   → 可复用思维模型和跨学科方法论
sources/    → 原始、未结构化数据底座
```
硬约束：Harness 拦截所有根目录越权写入。

## 核心口诀

> 描述负责路由，代码负责干活；断言卡死边界，裁判评估成果；路径严禁逾矩，大脑永不失忆。

## 关键洞见

- Skill = **为成本控制设计的文件夹结构**，非便利性。
- `context: fork` 防止读取大文件时主对话"失忆"。
- LLM-as-Judge 是评估主观输出的唯一可扩展方式。

## 关联

- [[Claude_Code_Skills]] - Skills 生态概览
- [[Harness_Engineering_Deep_Dive]] - Harness 工程深度
- [[Skill_Design_Patterns]] - 设计模式
- [[Skill_Ecosystem]] - 整体生态
- [[GBrain_Architecture]] - GBrain 架构中的 Fat Skills 模式

[Source: raw/10 Rules to create Skills.md]
