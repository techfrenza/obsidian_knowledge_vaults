---
title: Anthropic Agent SDK（Claude Code SDK）
aliases: ["Claude Code SDK", "Agent SDK", "Anthropic SDK"]
tags: [anthropic, sdk, agent, claude-code, api]
category: agent-engineering
parent: "[[Agent_Harness_Engineering]]"
created: 2026-05-15
date: "2026-05-15"
---

# Anthropic Agent SDK（Claude Code SDK）

Parent: [[Agent_Harness_Engineering]]
Source: [Source: raw/Anthropic Agent SDK（Claude Code SDK）.md]

## 定义
Anthropic Agent SDK（曾称 Claude Code SDK）将 Claude 的推理能力作为库直接集成到应用中，构建能**独立思考、使用工具并自我修正**的代理。与标准 Chat API 的核心差异：SDK 驱动[[Agentic_Loop]]，而非单次请求-响应。

## 核心架构：代理循环

| 阶段 | 说明 |
|------|------|
| 任务解析 | 接收用户指令，转化为可执行目标 |
| 工具选择 | Claude 自主决定调用哪些工具（Read、Bash、MCP 等） |
| 自主执行 | SDK 自动处理工具分发，开发者无需手写分发逻辑 |
| 观察与反思 | 根据工具返回的"地面事实"（Ground Truth）评估进度，动态调整策略，进入下轮循环 |

## 子代理系统（Subagents）
- **扁平化层级**：子代理之间平级，**不能嵌套**（子代理不能再派生子代理）
- **上下文隔离**：子代理拥有独立对话历史，主代理仅接收浓缩结果摘要
- **并行化**：主代理可同时派生多个子代理并行工作，效率高于顺序执行
- 详见 [[Claude_Code_Subagents]]

## 钩子机制（Hooks）
确定性控制平面，事件驱动，不消耗上下文 Token：
- **PreToolUse**：工具执行前拦截 → 权限校验、拦截危险命令
- **PostToolUse**：工具执行后触发 → 数据标准化、自动跑测试/格式化
- **UserPromptSubmit**：用户提交提示词时注入动态上下文（如最新 Git diff）
- 详见 [[Claude_Code_Hooks]]

## 权限与安全模型

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| `default` | 手动确认 | 开发调试 |
| `acceptEdits` | 自动改文件，命令需确认 | 半自动重构 |
| `bypassPermissions` | 全自动 | 仅限沙盒环境 |
| `plan` | 只读计划模式 | 架构规划 |

强烈建议在 Docker / E2B 等受控环境运行，防止误操作影响宿主机。

## 扩展能力
- **MCP**：通过三原语（工具、资源、提示词模板）连接外部数据库、Jira、Slack 等，见 [[MCP_Production_Agent]]
- **Agent Skills**：存储在 `.claude/skills/` 的 Markdown 文件，按需加载的任务级专业知识（SOP），仅在语义匹配时激活，节省上下文，见 [[Claude_Code_Skills]]

## 学习路径
1. 环境准备：Node.js 18+ 或 Python，`pip install claude-agent-sdk`
2. 编写第一个工具调用循环，观察 `stop_reason` 如何驱动状态流转
3. 配置 [[CLAUDE_md_Best_Practices|CLAUDE.md]]（金色原则 + 架构约束）
4. 通过 `effort` 参数（low/medium/high/max）调节推理深度；架构决策优先 Opus + Extended Thinking

## 核心原则
- 让 Claude 主导循环，不要手动编排 Prompt 链
- 大事化小：将大任务委托给特定领域子代理

## 导航
- [[Agent_Engineer_MOC]] — Agent Engineer 体系学习地图