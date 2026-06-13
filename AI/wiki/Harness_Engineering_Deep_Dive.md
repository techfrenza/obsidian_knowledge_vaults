---
title: Harness Engineering Deep Dive
aliases: ["线束工程深度解析", "Harness Engineering 定义", "AI 治驭基础"]
tags: [harness, agent, orchestration, reliability, context-management, evaluator-optimizer]
category: agent-engineering
parent: "[[Agent_Harness_Engineering]]"
created: 2026-05-08
date: "2026-05-08"
---

# Harness Engineering Deep Dive

Parent: Agent_Harness_Engineering
Source: [Source: raw/Harness Engineering.md]

> 核心论点：Harness Engineering 是一种范式转变——**模型是驾驶员，代码/环境是车辆**。目标是将 AI 自主性从"提示词赌博"（prompt gambling）提升至可控的 9/10 级可靠性。

---

## 定义

**Harness Engineering**：为 AI 模型在特定领域中有效运作而构建的**基础设施、工具与环境**的工程实践。

- 核心哲学：模型是 Agent（驾驶员），工程师创建的代码/环境是 Harness（车辆）
- 时间背景：到 2026 年被认定为软件工程的决定性因素，超越纯粹的 Prompt Engineering
- 终极目标：构建"高质量世界"让现有模型智能充分表达——模型本身已不是主要瓶颈

---

## 五大关键方法

### 1. System of Record（记录系统）
- 版本控制的文件：`AGENTS.md`（参见 [[AI_Team_Coding_Practice]]）、`CLAUDE.md`（参见 [[CLAUDE_md_Best_Practices]]）
- 存储：项目级指令、架构约束、"黄金原则"
- 对应实体：CLAUDE_md_Best_Practices

### 2. Evaluator-Optimiser 架构
```
Generator Agent → 生成输出
       ↓
Evaluator Agent → 基于结构化指标批评
  (架构一致性、事实准确性等)
       ↓
修正循环直到通过
```

### 3. 上下文治理（Context Governance）

| 机制 | 作用 |
|------|------|
| **Subagents** | 隔离复杂任务，防止主会话上下文污染 |
| **Recitation**（背诵） | 每个循环开始时强制 Agent 重述整体目标和当前计划，维持语义注意力，减少"目标漂移" |
| **Automatic Compaction** | 接近上下文限制时自动摘要/"垃圾回收" |

### 4. Physical Blocks（物理阻断）— Hooks
```json
// .claude/settings.json
{
  "hooks": {
    "PostEdit": ["run: npm run lint", "run: npm run typecheck"]
  }
}
```
强制执行：每次编辑后运行 linter/类型检查/单元测试 → 物理阻止不合规代码提交

### 5. 渐进式自主权（Incremental Autonomy）
```
阶段1：只读权限 → 观察成功循环
阶段2：写权限 → 观察稳定性
阶段3：Shell 权限 → 受控扩展
```
> 见 [[Human_In_The_Loop]] — 高风险阶段的 HITL 拦截门禁策略

---

## 优势与局限

### 优势
- **可靠性**：为 AI 输出提供"缰绳和围栏"，使其可预测、高质量
- **可扩展性**：3-7 人小团队交付百万行生产代码，零手写
- **Future-Proof**：底层模型升级时，Harness（规则、反馈循环、专业技能）依然有效
- **减少幻觉**：多代理 Evaluator-Optimiser 循环 + 强制验证关卡显著降低错误率

### 局限
- **上下文窗口约束**：过多或管理不善的上下文 → "lost in the middle" 效应
- **AI Slop 与架构漂移**：AI 生成速度过快 → 技术债，需要自动化"垃圾收集"或 Refactoring Agents
- **执行风险**：Shell/网络权限若未沙箱化和审计，可能导致系统损坏或数据泄露

---

## 真实案例

| 案例 | 描述 |
|------|------|
| **Claude Code** | Harness 典范：Bash/Read/Edit 工具 + CLAUDE.md 持久化上下文管理 |
| **OpenAI Codex 2026** | 小团队 + agentic harness → 100 万行生产代码，零人工手写 |
| **Matt Van Horn（前 Lyft 联创）** | `plan.md` 为"活 Harness"，80% 时间规划 + 20% 机械执行 × 多并行会话 |
| **Document Gardener** | 专用 Agent 定期扫描文档与代码行为，自动提交修复 PR |
| **非编码应用** | Farm Agents（传感器+灌溉工具集）、Hotel Agents（预订+管理 API） |

---

## 开放问题

1. **架构健康量化**：如何通过依赖图扫描精确计算"架构漂移分数"触发自动清理？
2. **RLDF 优化**：如何最高效地将人工 PR Review 转化为全组织可机械执行的"黄金原则"？
3. **工具标准化**：随着 Agent 开始管理金融交易，[[LangGraph_Deep_Agents|AP2（Agent Payments Protocol）]]如何集成到标准 Harness？参见 [[AI_Agent_Payments]] x402 协议的具体落地。

---

## Production Harness 的 12 个组件（完整定义）

来源：@akshay_pachaar《The Anatomy of an Agent Harness》—— 综合 Anthropic、OpenAI、LangChain、CrewAI

> "If you're not the model, you're the harness." — Vivek Trivedy (LangChain)

**冯诺依曼类比**：LLM = 无 RAM/磁盘/IO 的 CPU；上下文窗口 = RAM；外部数据库 = 磁盘；工具集 = 设备驱动；Harness = 操作系统。

| # | 组件 | 核心要点 |
|---|------|---------|
| 1 | **Orchestration Loop** | Thought-Action-Observation (TAO/ReAct) 循环；本身是"dumb loop"，复杂性在管理的内容 |
| 2 | **Tools** | Schema 注入 → Agent 知道什么能用；六类：文件/搜索/执行/Web/代码智能/子代理 |
| 3 | **Memory** | 短期（会话内）+ 长期（跨会话）；Claude Code 三层：index(150字) / topic files / raw transcripts |
| 4 | **Context Management** | Context Rot：关键内容落在窗口中段时性能下降 30%+；5种策略（压缩/遮蔽/JIT检索/结构记录/子代理委托） |
| 5 | **Prompt Construction** | 分层组装：系统提示 + 工具定义 + 内存文件 + 对话历史 + 用户消息 |
| 6 | **Output Parsing** | 原生 tool_calls 优于自由文本解析；Pydantic schema 约束输出 |
| 7 | **State Management** | LangGraph 类型化字典 + checkpointing；Claude Code = git commits + progress files |
| 8 | **Error Handling** | 10步流程 99% 成功率 = 90.4% 端到端成功率；4类错误：transient/LLM-recoverable/user-fixable/unexpected |
| 9 | **Guardrails & Safety** | 三层：input / output / tool；tripwire 机制；Claude Code 独立 40 个工具能力门禁 |
| 10 | **Verification Loops** | 规则验证（test/lint）+ 视觉验证（Playwright）+ LLM-as-judge；验证使质量提升 2-3× |
| 11 | **Subagent Orchestration** | Fork / Teammate / Worktree 三模式（Claude Code）；agents-as-tools / handoffs（OpenAI） |
| 12 | **Logging & Observability** | 轨迹评估 + trace-to-dataset 管道，用于持续改进 |

---

## 七大架构决策

| 决策 | 选项 | 建议 |
|------|------|------|
| 1 单 vs 多 Agent | Single vs Multi | 先最大化单 Agent；>10工具或明确独立域才拆分 |
| 2 ReAct vs Plan-and-Execute | 交错 vs 分离 | Plan-and-Execute 报告 3.6× 加速（LLMCompiler） |
| 3 上下文窗口策略 | 5种方案 | ACON：优先 reasoning trace > 原始工具输出，26-54% token 减少，精度 95%+ |
| 4 验证循环设计 | Guides（预测）vs Sensors（反馈） | 计算验证提供确定性基准；LLM-as-judge 捕获语义问题 |
| 5 权限与安全架构 | 宽松 vs 严格 | 依部署上下文选；默认严格（见 Human_In_The_Loop） |
| 6 工具作用域 | 多工具 vs 少工具 | 少往往更好：Vercel 删除 80% 工具后性能提升；lazy load 实现 95% context 减少 |
| 7 Harness 厚度 | Thin vs Thick | Anthropic 押注 Thin（模型升级自动变强）；Thin 的未来验证测试：更强模型不需加 harness 复杂度 |

---

## Harness Co-evolution 原则

模型被训练时 harness 在循环内 → 改变工具实现可能降低性能（紧耦合）。"未来验证测试"：如果更强模型不需要加 harness 复杂度就能提升性能 → 设计是对的。

---

## 三层持久化上下文资产（Anti-Rot 体系）

来源：Claude Code Harness Engineering 指南 / 全面最佳实践综述

**核心原则**：上下文不是临时对话，而是**可复利的资产系统**。

| 文件 | 角色 | 内容范围 |
|------|------|---------|
| `CLAUDE.md` | 硬性规约（地图，不是说明书） | 架构规则、团队标准、硬性约束（如"严禁未授权修改支付模块"）；指向更深层的真相，而非自包含 |
| `AGENTS.md` | 能力地图 | Agent 如何在该仓库工作、构建、测试，以及如何发现工具包 |
| `DECISIONS.md` | 决策日志 | 架构选择、被拒绝方案、已知 bug 模式；防止 Agent 在后续会话中重复错误路径 |

详见 AI_Team_Coding_Practice — 三文件体系的写法规范与维护节奏。

### 分层加载与渐进式披露
- 根据**目录距离**加载规则：离当前工作目录越近的规则优先级越高
- `MEMORY.md` 索引化：作为指向具体 Topic 文件的**一行指针列表**，入口文件必须短小（否则拖慢响应 + 上下文膨胀）
- `.claudeignore`：排除 `node_modules`、构建产物、日志文件，减少上下文噪声

### Anti-Rot 策略
- **主动压缩**：Token 使用量达 60% 时 `/compact`，保留架构决策和未解决 Issue，丢弃冗余工具输出（见 [[Claude_Code_Commands]]）
- **保留错误信息**：出错时保留完整失败调用和堆栈追踪，让模型"吃一堑长一智"，不要清除（见 [[Unique_Engineering_Insights]] 错误是"主路径"而非"例外"）
- **Compound 沉淀**：任务结束输入"总结本会话决策和教训，更新至 DECISIONS.md"（见 AI_Team_Coding_Practice 四步闭环）

---

## 与其他笔记的联系

- Agent_Harness_Engineering — 上层 Harness 工程体系（父页面）
- [[Claude_Code_Hooks]] — Physical Blocks 的具体实现
- [[Claude_Code_Subagents]] — Context Governance 之 Subagent 隔离
- CLAUDE_md_Best_Practices — System of Record 实践
- [[Context_Engineering]] — 上下文治理的完整框架
- [[Enterprise_AI_Architecture]] — Harness 在企业级架构中的位置
- [[Multi_Agent_Architecture]] — Evaluator-Optimiser 多代理协作
- AI_Team_Coding_Practice — `AGENTS.md` 规范在团队 Harness 中的写法与边界
- [[Claude_Code_Advanced_Features]] — §9 大型代码库 7 组件 Harness 优先级表与 LSP/Plugin 企业部署实操
- LangGraph_Deep_Agents — AP2 协议与 Harness 金融工具标准化
- Unique_Engineering_Insights — "Harness > 模型"实证数据与 Skeptical Evaluator 模式
- [[Agentic_Loop]] — Harness 内层循环的 ReAct 细节与成本风险分析
- [[GBrain_Architecture]] — GBrain = Fat Skills + Thin Harness 的完整个人级落地（Thin Harness 路由层 + Fat Skills + 100k 页知识库）
- [[GBrain_Fat_Thin_Architecture]] — Fat Skills + Thin Harness 架构详解：四层架构、Skillify 元技能、Book-Mirror 技能示例

*[Source: raw/Harness Engineering.md, raw/The Anatomy of an Agent Harness.md, raw/Claude Code Harness Engineering 指南.md, raw/Claude Code 的全面最佳实践指南.md]*

- [[Harness_Engineering_Advanced]] — Harness 进阶：三层持久化上下文/Plan-First 工作流/并行编排/熔断机制
