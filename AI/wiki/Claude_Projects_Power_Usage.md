---
title: "Claude Projects Power Usage"
parent: "[[Claude_Cowork]]"
tags: [claude-projects, persistent-context, knowledge-base, living-instructions]
category: claude-tooling
date: "2026-06-07"
source: "raw/25 Claude Features, Workflows, and Tricks That Most Users Don't Know.md"
---

# Claude Projects Power Usage

Claude Projects 是最被低估的持久化功能：让 Claude 拥有**长期记忆 + 领域专精 + 持续进化**的能力，而不是每次新对话从零开始。大多数用户只利用了约 10% 的能力。

[Source: raw/25 Claude Features, Workflows, and Tricks That Most Users Don't Know.md]

---

## 核心机制

**Project Instructions = 永久 System Prompt**：写一次，自动应用于 Project 内所有对话。节省每次会话开头 2-5 分钟的重复说明。

**结构化 Instructions 模板**：

```
ROLE: You are my [具体角色] specializing in [具体领域].
CONTEXT: I work at [公司/角色]. My audience is [谁]. My current priorities are [什么].
RULES:
- Always [具体行为]
- Never [具体要避免的事]
- When uncertain, [如何处理]
OUTPUT DEFAULTS:
- Tone: [具体语气]
- Length: [默认长度]
- Format: [输出结构偏好]
```

---

## 设置 + 配置阶段（技巧 01-07）

| 技巧 | 核心操作 | 价值 |
|------|---------|------|
| 01 | Project Instructions 作为永久 System Prompt | 零重复说明 |
| 02 | 用结构化模板（ROLE/CONTEXT/RULES/OUTPUT DEFAULTS）写 Instructions | 比散文段落更稳定 |
| 03 | 上传全部相关文件（文档/风格指南/SOP/历史报告） | Claude 从你自己的文档回答，非通用知识 |
| 04 | 文件命名描述性强（`refund-policy-2026.md` 而非 `doc1.pdf`） | Claude 用文件名判断相关性 |
| 05 | Living Instructions 模式：每次输出不达标时新增规则 | 形成正向进化循环 |
| 06 | 按领域拆分多个 Project（内容/客户/研究/个人） | Claude 成为各领域专家 |
| 07 | Starter Conversation 校准：让 Claude 总结已知内容并生成样例输出 | 15 分钟防止数周次优输出 |

---

## 日常工作流（技巧 08-14）

- **Context-Rich Question**：Project 内直接问裸问题（"3年承诺折扣建议？"），无需重复背景，因为 Project 已知一切
- **Conversation Chain**：每个任务开新对话，自然引用之前内容（上下文隔离 + 知识复用）
- **Research Accumulator**：持续 "Add this to my knowledge base on [topic]"，几周后形成深度合成知识
- **Template Generator**：从最佳历史输出提炼可复用模板，基于真实工作而非通用格式
- **Quality Audit（元优化）**：定期让 Claude 审查自己的 Project 配置，提出冲突指令/过时文件/知识空白/建议规则

---

## 进阶技巧（技巧 15-21）

- **Voice Calibration File**：上传带标注的写作样本（`my-writing-voice.md`），Claude 匹配具体模式而非解读抽象形容词
- **Client-Specific Projects**：自由职业者/咨询师为每位客户单独建 Project
- **SOP Builder**：从对话历史提炼标准作业流程（基于实际工作而非理想化步骤）
- **Feedback Loop Logger**：输出不理想时明确指出问题，让 Claude 建议 Instructions 更新
- **Seasonal Refresh**：每季度审计 Instructions 准确性、文件时效性、Project 必要性

---

## Power User 秘密技巧（技巧 22-25）

**Instruction Priority System**（解决长指令集冲突）：
```
CRITICAL RULES (never violate): 1. [最重要] 2. [次重要]
STANDARD RULES (follow unless overridden): 3-10.
PREFERENCES (apply when relevant): 11-15.
```
不设优先级时，Claude 平等对待所有规则，可能用次要规则覆盖核心规则。

**Persona Switching**：临时覆盖默认语气（"这次对话用 skeptical investor 视角"），Project 上下文保持不变。

**Benchmark Conversation**：创建一个黄金标准输出的对话，后续输出与其对比。

**Compounding Knowledge Strategy**：每次对话都是对未来的投资。6个月后精心维护的 Project 与全新对话的差距是质变级的。

---

## 与其他概念的关系

- [[Claude_Cowork]] — Projects 是 Cowork 的核心基础设施层
- [[CLAUDE_md_Best_Practices]] — Projects Instructions 与 CLAUDE.md 的关系：前者是 UI 层持久化，后者是文件级持久化
- [[Agentic_Memory_System]] — Projects 实现 Claude 用户界面的 External Memory
- [[Karpathy_Methodology]] — Living Instructions 与 Karpathy Loop 的哲学对应：让系统随实践自我进化
- [[Instruction_Sharing]] — 跨 Project/Session 的指令共享策略
- [[Cross_Platform_Memory]] — Projects Knowledge Base 是跨平台记忆的 Claude UI 实现
