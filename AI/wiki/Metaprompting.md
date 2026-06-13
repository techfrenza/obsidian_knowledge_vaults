---
title: 元提示工程（Metaprompting）
parent: "[[Prompt_Engineering_Advanced]]"
tags: [metaprompting, prompt-engineering, iterative, x-platform]
category: prompt-engineering
stub: false
date: "2026-06-03"
---

# 元提示工程（Metaprompting）

**普通 Prompt** = 直接问 AI 问题  
**Metaprompting** = 让 AI 帮你生成、批判、迭代提示本身，把一次对话变成"提示的进化循环"。

目标：把 v1 普通提示迭代成 v10、v27 这样越来越强的"超级提示"。

## Metaprompting 循环（四步法）

### Step 1：写 v1 普通 Prompt（大多数人停在这里）
```
帮我写一条关于「Obsidian + Claude 第二大脑」的 X 帖子，要有吸引力。
```

### Step 2：进入 Metaprompting 循环
将 v1 丢给 Claude，要求生成"超级提示"，并指定：
1. **角色设定**：具体专业角色（如硅谷顶级增长黑客）
2. **明确目标**：输出的成功标准（如 bookmarkable content）
3. **判断标准**（Rubric）：可量化的评判维度
4. **输出格式**：结构化要求
5. **语气风格**：直接、反鸡汤、实用
6. **长度约束**：信息密度要求

### Step 3：得到 v2 超级提示
AI 生成包含所有维度的结构化超级提示模板。

### Step 4：继续迭代（v3、v4…）
批判上一版不足，针对性优化：
- 核心差异点是否突出
- 语气是否足够尖锐
- 是否缺少特定框架（如时间线效果：1个月/3个月/6个月）

## X 平台内容的 Bookmarkability Rubric

高质量 X 帖子必须满足：
- **节省时间**：让读者跳过学习曲线
- **可复用**：给出框架或步骤
- **带例子**：具体而非抽象
- **立即执行**：读完能马上做

## 为什么 Metaprompting 优于手写 Prompt

| 对比维度 | 手写 Prompt | Metaprompting |
|---------|------------|---------------|
| 覆盖维度 | 只想到自己能想到的 | AI 补全遗漏维度 |
| 迭代速度 | 线性（一次一次试）| 指数（批判+重写）|
| 可重复性 | 低（每次重写）| 高（模板化）|
| 专业性 | 受限于个人知识 | 站在领域最佳实践上 |

## 实战洞见

- **Rubric 是关键**：判断标准越具体，AI 自我评估越准确。
- **角色 + 目标 > 步骤**：给 AI 角色和目标比给步骤效果更好。
- **批判优先**：每次迭代先明确"这版的 3 个问题"再要求重写。

## Prompt Folding（进阶形式）

Metaprompting 的升级版：提示词根据上下文、历史查询或分类结果**自动生成专属子提示词**。静态提示 → 自我进化/分支生成的系统。

### 核心机制：Classifier + Dynamic Sub-Prompt

**Step 1：分类器 Prompt**
```
分类用户查询为：Research / Writing / Coding / Analysis / Brainstorming / Planning / Other
然后为该分类生成 Specialized Sub-Prompt（包含：角色设定/具体任务/输出格式/判断标准/思考风格）
```

**Step 2：动态生成专属子提示**
同一通用提示 → 写作查询 → 生成"硅谷增长黑客"写作提示；研究查询 → 生成"分析师"研究提示。

### 进阶：多层折叠（递归）
- 复杂任务（Research/Planning）→ 继续生成第2层子提示
- 分解为子任务 → 每个子任务生成专用提示
- 带历史记忆的折叠：在 CLAUDE.md 中写入"每次用户输入后，先回顾最近3次历史，再用 Classifier 生成最优子提示"

### Prompt Folding vs Metaprompting

| 维度 | Metaprompting | Prompt Folding |
|------|--------------|----------------|
| 机制 | 人工批判迭代 | 自动分类+生成 |
| 适用 | 单一任务优化 | 多类型任务路由 |
| 自动化程度 | 低 | 高 |
| 特别适合 | 内容创作优化 | Agent 工作流 |

## 关联

- [[Prompt_Engineering_Advanced]] - 高级提示工程
- [[Prompt_Template_Library]] - 提示词模板库
- [[Research_Prompts]] - 研究提示词
- [[Context_Engineering]] - 上下文工程（比 Metaprompting 更底层）
- [[Claude_Optimization]] - Claude 优化
- [[Skill_Design_Patterns]] - Skill 设计中的 Classifier 模式

[Source: raw/Meta prompting.md]
