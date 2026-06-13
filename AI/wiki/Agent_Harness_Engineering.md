---
title: Agent Harness Engineering
aliases: ["线束工程", "Harness Engineering", "AI 系统治驭"]
tags: [harness, agent, orchestration, context-management, subagents, scaling]
category: agent-engineering
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Agent Harness Engineering

Parent: [[index]]

> 核心论点：Claude Code 的性能不取决于模型能力，而取决于其运行的"线束"环境。将模型视为**不稳定的工程部件**，通过制度化的控制平面确保可靠、可重复的工程行为。

---

## 五层 TAO/ReAct 核心循环

```python
while not done:
    prompt = assemble_prompt(system, tools, memory, history, user_msg)
    output = llm.call(prompt, effort="xhigh")
    if no_tool_calls(output):
        final_answer = output.text
        break
    else:
        results = execute_tools(output.tool_calls)
        history.append(results)
        if near_context_limit(): compact_history()
```

---

## 上下文治理三原则

### 防 Rot 四招（Context Compaction）
| 策略 | 触发点 | 效果 |
|------|--------|------|
| Compaction | 窗口 60-70% 时 | 保留架构决策+未解决 Bug |
| Observation masking | 旧 tool result | 只保留 tool calls |
| JIT retrieval | 文件读取 | 用 grep/glob 代替全文件加载 |
| Sub-agent delegation | 高输出任务 | 子代理返回 ≤2000 token 摘要 |

### Prompt 构建严格优先级
```
server system prompt
→ tool schemas
→ CLAUDE.md/AGENTS.md
→ conversation history
→ current user message
```

---

## 三维度 Scaling 决策框架

每次扩展前问：问题是 **time**（长时运行漂移）、**space**（并行多 agent 瓶颈）还是 **interaction**（人工介入低效）？

| 维度 | 架构模式 |
|------|----------|
| Time Scaling | 三角色架构：Planner + Generator + Evaluator |
| Space Scaling | 递归 Planner-Worker + 隔离 repo |
| Interaction Scaling | ticket-driven + WORKFLOW.md |

---

## 错误处理四分类

| 类型 | 处理策略 |
|------|----------|
| Transient | backoff retry |
| LLM-recoverable | 作为 ToolMessage 返回给 model 自纠 |
| User-fixable | interrupt 等待 human input |
| Unexpected | bubble up + post to #dev-alerts |

最多 retry 2 次，失败结果继续喂 loop 让 model 调整。

---

## 容错恢复层级（Recovery Ladder）

遇到失败按层级升级，禁止无限循环：

| 异常 | 处理顺序 |
|------|----------|
| **Prompt Too Long** | ① Context Collapse（提交积压折叠信息）→ ② Reactive Compact |
| **Output Truncation** | 追加 Meta User Message："直接续写，不要道歉，不要总结" |
| **连续 autocompact 失败** | 熔断（≥3 次停止），防止无限循环烧 API 预算 |

---

## Skill 封装最佳实践

每个 Skill = 含 `SKILL.md` 的文件夹（YAML frontmatter 定义触发条件）

- **渐进式披露**：Agent 认为技能相关时才加载详细指令，节省 Token
- **Gotchas 模块**：在 Skill 文件中维护"避坑指南"，记录过往失败案例
- **`CLAUDE_CODE_FORK_SUBAGENT=1`**：让子代理继承父会话上下文和 Prompt 缓存（与 `/fork` 等效）

> 关联：[[Claude_Code_Skills]] — Skill 插件系统完整设计模式

---

## 七个架构决策

1. **Single vs multi**：优先 single agent
2. **ReAct vs Plan-and-execute**：复杂任务用 Plan-and-execute（LLMCompiler 实测 3.6x speedup）
3. **Context strategy**：structured note-taking + sub-delegation（token 减 26-54%）
4. **Verification**：computational（确定性）优于 inferential
5. **Safety**：restrictive 模式（高风险需 explicit confirm）
6. **Tool scoping**：最小必要集 + lazy loading（≤10 个工具）
7. **Harness thickness**：模型升级后定期删 planning 步骤

---

## 三种 Claude Code 子代理模式

| 模式 | 适用场景 |
|------|----------|
| Fork（全 copy context）| 继承现有理解，独立推进 |
| Teammate（terminal pane + mailbox）| 协同工作 |
| Worktree（独立 git branch）| 隔离文件修改 |

---

## 关联实体

- [[Agent_Context_Architecture]] — Context 的四层分区设计
- [[Claude_Code_Skills]] — Harness 的 Skill 封装层（含 Gotchas 模块）
- [[Claude_Code_Hooks]] — Harness 的确定性约束层
- [[Claude_Code_Subagents]] — 子代理编排模式（含 Fork/CLAUDE_CODE_FORK_SUBAGENT 继承）
- [[MCP_Production_Agent]] — 跨工具连接的神经系统层
- [[LangGraph_Deep_Agents]] — LangGraph 运行时与 Deep Agents 组件包
- [[index]] — 主索引
- [[Anthropic_Agent_SDK]] — Anthropic 参考 harness 实现
- [[Unique_Engineering_Insights]] — Harness > 模型的实证与非直觉洞见
- [[Agent_Engineer_Roadmap]] — Phase 3 自建 1500 行 mini-harness
- [[Harness_Engineering_Deep_Dive]] — 完整定义、5 大方法、真实案例与开放问题
- [[Multi_Agent_Missions_System]] — Factory Missions 三角色架构（Orchestrator/Workers/Validators）与 Harness 长期任务编排的直接关联

*[Source: raw/Agent Harness.md, raw/AI Orchestration Practical Knowledge 围绕AI构建系统.md, raw/Claude Code 系统治驭工程指南.md, raw/Claude Code Harness Engineering 指南.md]*
