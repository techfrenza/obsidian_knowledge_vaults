---
title: Claude Code Commands
aliases: ["Claude Code 命令", "35 个技巧", "日常命令速查"]
tags: [claude-code, commands, productivity, shortcuts, workflow]
category: claude-tooling
parent: "[[index]]"
created: 2026-04-30
date: "2026-04-30"
---

# Claude Code Commands

Parent: [[index]]

> 核心论点：Claude Code 的命令体系是生产力的乘法因子。掌握 8 个核心命令可立即提升 80% 效率；并行会话 + 任务原子化是高效编码的根本心智模型。

---

## Essential Commands（每天必敲）

| 命令 | 用途 |
|------|------|
| `Shift + Tab` | 进入 Plan Mode：先让 Claude 分析输出架构计划，审查后再切回实现 |
| `/compact` | 会话 30-45 分钟后压缩历史为关键决策摘要 |
| `/clear` | 新任务前清空所有上下文（一功能一会话）|
| `/init` | 新项目启动：自动扫描生成 [[CLAUDE_md_Best_Practices|CLAUDE.md]] |
| `/cost` | 每小时查看 token 消耗 |
| `/memory` | 添加跨 session 永久生效的规则 |
| `! 前缀` | 直接执行终端命令，不切换窗口（如 `!git status`）|
| 模型切换 | 规划/架构用 Opus，执行/实现用 Sonnet |

---

## 上下文治理命令

| 命令 | 触发时机 |
|------|----------|
| `/compact` | token 达 60-70% 时主动压缩 |
| `/clear` | 完成独立任务后（防脏上下文传染）|
| `/context` | 实时查看 token 消耗 |
| `/rewind` | 回溯 checkpoint 重新总结 |
| `双击 ESC` | 回溯修改上一条输入 |
| `ESC` | 中止跑偏的执行 |

---

## Productivity Techniques（直接套用）

- **Reference File**：不说风格，直接说 "Look at `src/auth/login.ts`，用完全相同 pattern 实现 password reset"
- **Screenshot Debug**：UI 问题直接 Ctrl+V 贴截图
- **Test-First**："Write tests for [function]，cover [all edge cases]，then implement to pass all tests"
- **Incremental Build**：大功能拆成 "Create DB schema → Test → Build API → Test → Add validation → Test"
- **Error Paste**：贴完整 error + stack trace + "Diagnose root cause step by step before fix"
- **Undo Checkpoint**：大改前先 `git commit -m "checkpoint before [change]"`

---

## 六大心智模型

| 模型 | 核心操作 |
|------|----------|
| Junior 同事 | 描述现象，让 Claude 判断原因（减少预设答案）|
| 60% compact | 超 60% 立即 /compact，不是省 token，是注意力重聚焦 |
| Fail fast ESC | 看到跑偏立即 ESC，重开成本 < 修正成本 |
| Sub agents 隔离 | 调研/搜索/扫描用 subagent，主线保持干净 |
| Haiku for simple | 1-2 步认知任务用 Haiku；10 步以上用 ultrathink |
| /loop for monitoring | CI 检查、慢查询扫描、PR review 队列 |

---

## 6 层架构诊断（卡住时按层检查）

```
Layer1: 底层循环（context 加载顺序）
Layer2: [[MCP_Production_Agent|MCP]]/Tools（工具定义 token 开销）
Layer3: [[Claude_Code_Skills|Skills]]（按需加载工作流）
Layer4: [[Claude_Code_Hooks|Hooks]]（强制确定性约束）
Layer5: [[Claude_Code_Subagents|Subagents]]（隔离执行）
Layer6: Prompt Caching + Verification（缓存命中+闭环校验）
```

---

## 5 分钟项目启动流程

1. 运行 `/init` 生成 CLAUDE.md
2. 把 coding standards 和 patterns 加进 CLAUDE.md
3. `/memory` 添加永久规则
4. `Shift + Tab` 进 Plan Mode 规划架构
5. Incremental + Test-First 逐小步构建

---

## 高级命令与技巧

| 命令 / 技巧 | 用途 |
|------------|------|
| `/ultraplan` | 30 分钟高强度深度思考，用于极难架构问题 |
| `claude --permission-mode auto` | Team+ 计划：第二 AI 作安全分类器，低风险自动放行 |
| `claude remote-control` | 手机/远程监控+控制本地运行的重型开发任务 |
| `cat logs.txt \| claude -p "..."` | 管道输入：将外部日志/diff 直接喂给 Claude |
| `claude "任务描述"` | 干净上下文直接启动（带任务启动，避免污染）|
| `.claudeignore` | 排除 node_modules/构建产物/日志，减少上下文噪声 |

## Level 4+ 专属命令（Boris Cherny 生产实践）

| 命令 | 用途 |
|------|------|
| `/btw` | 任务进行中问一个快问题（不打断主流程，不污染 session 历史）|
| `/branch`（旧称 `/fork`）| 在当前对话精确节点创建分叉，一个分支试一种方案，随时跳回——对话级 Git |
| `/insights` | 分析过去一个月使用模式：高重复操作、token 浪费点、应升级为 Skill 的 prompt |
| `/output-style new` | 切换 Claude Code 输出风格（内置：default/explanatory/learning；支持自定义描述，如 code-reviewer/no-fluff 模式）|
| `/focus` | 隐藏中间步骤，只显示最终结果（与 Auto Mode 配合，Boris 5 并行 session 核心工具）|

**Opus Plan 模式**（Level 4 隐藏设置）：Opus 负责规划，Sonnet 负责执行。聪明模型用于决策，廉价模型用于实现——质量不降，成本减半。详见 [[Opus_4_7_Migration]] 模型选择策略。

## Task Budgets（Opus 4.7 Beta API）

给 agent 设定 token 预算目标（思考 + 工具调用 + 输出均计入）。模型自感知预算，任务接近上限时优雅收尾，而非撞墙中断。**生产级 Agent 成本控制的核心杠杆**（当前仅 API 可用，Claude Code/Cowork 暂未支持）。与 [[AI_Agent_247_Architecture]] 的熔断机制形成双保险：Task Budgets 控制单次成本，熔断控制累计成本。

*[Source: raw/Every Level of Claude Explained (After 400+ Hours Inside).md]*

---

## 四阶段闭环循环（Plan → Execute → Verify → Repair）

```
Phase 1: Plan    → Shift+Tab 进 Plan 模式，输出步骤+架构影响+测试策略
Phase 2: Execute → 人工审查 Plan 后切回实现
Phase 3: Verify  → 跑测试/Linter 证明变更有效（不能只听"已完成"）
Phase 4: Repair  → 测试失败 → 立即修复，不跳过
```

> 手术式修改原则（Karpathy Rule）：只改用户指定的代码，拒绝未请求的功能，保持方案最简化。

---

## 关联实体

- [[CLAUDE_md_Best_Practices]] — 每次启动自动加载的规则文件
- [[Claude_Code_Skills]] — `/skill-name` 触发工作流
- [[Claude_Code_Hooks]] — 自动执行的确定性约束
- [[Claude_Code_Subagents]] — 上下文隔离执行层
- [[Agent_Harness_Engineering]] — 完整六层架构框架
- [[Prompt_Engineering_Library]] — `/clear` + 手写 brief 时引用的结构化提示模板库（40 个专家级 Prompt 分类）
- [[Claude_Code_CLI_Reference]] — 完整 CLI flags/斜杠命令/启动命令速查手册

*[Source: raw/Claude Code commands.md, raw/Best practice to use Claude code.md, raw/Claude Code 的全面最佳实践指南.md]*

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图