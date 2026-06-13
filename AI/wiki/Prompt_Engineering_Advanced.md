---
title: Prompt Engineering Advanced（元提示工程）
aliases: ["Metaprompting", "Prompt Folding", "元提示", "提示词折叠"]
tags: [prompt-engineering, metaprompting, prompt-folding, classifier, iteration]
category: prompt-engineering
parent: "[[Prompt_Engineering_Library]]"
created: 2026-05-11
date: "2026-05-11"
---

# Prompt Engineering Advanced（元提示工程）

Parent: [[Prompt_Engineering_Library]]

> 核心论点：普通 Prompt = 一次性指令；Metaprompting = 让 AI 帮你生成、批判、迭代 Prompt 本身，把一次对话变成"提示的进化循环"。Prompt Folding 是其进阶形式——提示词自我分叉，根据上下文动态生成专属子提示。

[Source: raw/Meta prompting.md, raw/Meta-Meta-Prompting The Secret to Making AI Agents Work.md]

---

## Metaprompting

### 定义与区别

| 类型 | 描述 |
|------|------|
| 普通 Prompt v1 | 直接问 AI 问题，一次性使用 |
| Metaprompting | 让 AI 生成/批判/迭代 Prompt 本身 |
| 结果 | v1 → v10 → v27 的"超级提示"进化循环 |

### 操作流程（4步）

**Step 1**：写 v1 普通 Prompt（大多数人止步于此）

**Step 2**：进入 Metaprompting 循环
```
请你为我生成一个超级提示（meta prompt），要求如下：
输入：[主题] + [我的写作风格]
输出：[目标输出]
要求：
1. 角色设定：[领域顶级专家]
2. 明确目标：[bookmarkable/可交付标准]
3. 判断标准（rubric）：[节省时间/可复用/带框架/带例子]
4. 输出格式：[具体格式]
5. 语气：[直接/实用/反鸡汤]
6. 长度限制：[上限]
请直接输出最终版超级提示。
```

**Step 3**：得到 v2 超级提示（AI 生成）

**Step 4**：继续批判迭代
```
这个提示还有3个问题：
1. [缺失的核心点]
2. [语气问题]
3. [缺少的框架]
请基于以上反馈，生成 v3。
```

### 通用模板
```
请你为我生成/优化一个 meta prompt，要求如下：
目标任务：[你想让AI帮你做什么]
输入信息：[你会给AI什么]
输出要求：
- 具体格式
- 长度限制
- 语气风格
- 判断标准（rubric）
- 必须包含的元素
我的思考风格：[直接/注重compounding/讨厌鸡汤]
请直接输出最终优化后的超级提示。
```

---

## Prompt Folding（提示词折叠）

### 定义

Prompt Folding = Metaprompting 进阶形式：让提示词**自我进化或分支生成**更精准的专属子提示，根据上下文动态适配。

| 类比 | 含义 |
|------|------|
| 普通 Prompt | 固定菜单点菜 |
| Metaprompting | 让厨师帮你写更好的菜单 |
| Prompt Folding | 菜单根据客人当天口味自动生成专属子菜单 |

特别适合：Agent 工作流、多阶段任务、个性化流程

### 实现方式：Classifier + Dynamic Sub-Prompt

**第一步：Classifier Prompt（分类器）**
```
你是一个极度精准的 Prompt 分类器 + 路由专家。
用户输入：[用户查询]
请执行：
1. 准确分类（只选一个）：
   - Research / Writing / Coding / Analysis / Brainstorming / Planning / Other
2. 根据分类生成高度专业化的子提示词（sub-prompt）
   - 子提示必须包含：角色设定、具体任务、输出格式、判断标准、思考风格
   - 必须极度针对查询细节
输出格式：
Classification: [类别]
Specialized Sub-Prompt:
"""
[完整优化后的子提示词]
"""
```

**第二步**：用生成的 Sub-Prompt 执行任务

### 进阶：多层折叠

```
如果任务复杂（Research 或 Planning），请继续生成第2层子提示：
- 先分解成子任务
- 为每个子任务生成专用提示
- 输出格式：Stage 1 Prompt → Stage 2 Prompt → ...
```

### 带历史记忆的折叠（最强版）

在 CLAUDE.md 或系统提示中加入：
> "每次用户输入后，先回顾最近3次交互历史，再用 Classifier 生成当前最优子提示。"

### 通用 Prompt Folding 模板
```
你现在是 Prompt Folding 专家。
目标：把用户输入转化为最高效的执行提示。
用户输入：[INSERT USER QUERY HERE]
执行：
1. 分析用户真实意图和难点
2. 分类任务类型
3. 生成 v2 优化版 Specialized Prompt（比原输入强3倍以上）
4. 如果需要，生成多阶段折叠提示
输出格式：
Classification:
Specialized Prompt v2:
"""[完整提示]"""
```

---

## 与 GBrain Skillify 的关联

Garry Tan 的 GBrain `/skillify` 命令，本质上是 Metaprompting 的系统化实现：
- 观察工作流 → 用对话提炼"超级提示（Skill）" → 测试验证 → 注册到 resolver
- 区别在于 GBrain Skill 有测试套件和 eval 回路，而基础 Metaprompting 只有迭代对话

详见 [[GBrain_Architecture]] §Skillify

---

## Claude Opus 4.8 专项提示优化（官方 2026-06 指南）

[Source: raw/Prompting best practices.md]

### Effort 层级对提示策略的影响

| Effort 等级 | 适用场景 | 注意点 |
|------------|----------|--------|
| `xhigh` | 编码、Agentic 最佳默认 | 大幅增加 tool call 和子 agent 使用 |
| `high` | 智能敏感通用场景底线 | 平衡 token 与智能 |
| `medium` | 成本敏感，接受智能降低 | 复杂推理风险 under-thinking |
| `low` | 短任务、延迟敏感 | 字面执行，不自动推广指令范围 |

**关键规则**：Claude Opus 4.8 在低 effort 下字面遵守指令——`"Apply this to every section, not just the first one"` 必须明确写出范围，模型不会自动推广。

### 代码审查 Harness 调优

旧 Harness 在 Opus 4.8 上可能出现 recall 下降：模型深度调查后因"conservative"指令过滤掉低 severity 发现。**解决方案**：分离 coverage 阶段（所有发现含 confidence 级别）与 filter 阶段，不在同一 pass 内合并。推荐 prompt：
```
Report every issue you find, including ones you are uncertain about or low-severity.
Do not filter for importance at this stage. Include your confidence level and severity for each finding.
```

### Design Prompt（前端 UI）

Claude Opus 4.8 默认设计风格：暖米色背景（~#F4F1EA）、衬线字体（Georgia/Fraunces）、赤陶/琥珀色调。非编辑/出版场景需覆盖：
1. 提供具体颜色规范（hex 值），比"don't use cream"有效
2. 或先让模型提出 4 个方向后用户选择（打破默认风格定势）

### Subagent 生成控制

Opus 4.8 默认生成更少子 agent（比 4.7 保守）。需要 fan-out 时明确声明：
```
Spawn multiple subagents when fanning out across items or reading multiple files.
Do not spawn a subagent for work you can complete directly in a single response.
```

---

## 关联概念

- [[Prompt_Engineering_Library]] — 40 个专家级即用 Prompt 模板（上级）
- [[Prompt_Template_Library]] — 即用模板完整分类列表
- [[GBrain_Architecture]] — Skillify = 系统化 Metaprompting，带测试闭环
- [[Claude_Code_Skills]] — Skill description 写法 = 应用级 Prompt Folding（触发路由）
- [[Context_Engineering]] — 提示质量上限由上下文语义质量决定
- [[Metaprompting]] — Metaprompting 四步法详细展开（X 帖子内容创作场景）
- [[Claude_Code_CLI_Reference]] — Effort flag 的 CLI 设置方式
- [[Opus_4_7_Migration]] — 从 4.7 迁移到 4.8 的参数变化
