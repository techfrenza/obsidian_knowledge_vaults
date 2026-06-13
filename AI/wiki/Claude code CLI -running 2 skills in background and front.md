---
title: Claude Code CLI 并行技能运行
aliases: ["后台技能", "并行Skills", "background skill", "前后台并行"]
tags: [claude-code, cli, background, parallel, skills]
category: claude-tooling
parent: "[[Claude_Code_Skills]]"
created: 2026-05-15
date: "2026-05-15"
---

在 2026 年最新的 Claude Code CLI 生态中，将上述的“基础 Bash 命令后台化”与“高级 AI 代理/技能管理机制”深度结合，可以提炼出一套面向未来的全场景并行开发专家指南。

当你想在 Claude Code 中一边运行技能 Y（如主线编码、重构），一边让技能 X（如代码扫描、测试流、环境部署、自动化调研）在后台执行，可以根据任务性质选择以下 5 种最佳实践：

### 1. 终端级：手动将技能 X 移至后台（针对 Bash 命令/基础脚本）

如果你已经通过 Claude 启动了技能 X（例如运行一个持续监听的测试服务或启动开发服务器），你可以立即释放当前 CLI 窗口去执行任务 Y。

- 快捷键一击即中：在终端执行技能 X 的 Bash 过程中，直接按下 Ctrl + B。当前挂起的子进程或常规工具调用会被立即推入后台（你可以通过 Ctrl + T 随时切换/隐藏底部的任务状态面板）。
    
- 自然语言先发制人：在下达指令时直接说明：“在后台启动技能 X（如：npm run test:watch），然后我们来做任务 Y。” 此时 Claude 调用底层 Bash 工具时会自动带上后台运行参数，绝不阻塞当前输入框。
    

### 2. 架构级：通过 SKILL.md 配置实现自动解耦与后台化

如果你正在编写一个团队常用的自定义技能 X，并希望它被调用时默认就在后台异步执行，或者不污染当前主对话的 Token 上下文，可以在该技能的 SKILL.md 文件的 YAML Frontmatter 中进行声明：

---  
name: security-scanner  
description: Run code vulnerability scan in the background  
# 2026 最新标准配置  
context: fork          # 将技能放入独立的 Fork 代理中运行，与主会话上下文完全隔离  
background: true       # 允许该子代理作为后台任务挂起，绝不阻塞主会话  
allowed-tools: [read_file, write_file, bash]  
---  
  

- 带来的优势：当你输入 /security-scanner 触发技能 X 时，Claude 会自动分流（Fork）出一个默默无闻的数字员工去干活。此时你的主线对话完全干净，你可以立刻输入任何指令让 Claude 协同你搞定任务 Y。
    

### 3. 工程级：利用 Git 工作树（Worktrees）实现真正的多任务并行

当技能 X 和技能 Y 都属于写操作（都需要修改代码文件），如果让它们在同一个目录下并行，会引发严重的“文件冲突”或“代码覆盖”。2026 年 Claude Code 官方内置了强大的 Git 工作树隔离机制。

- 启动独立会话：在另一个终端窗口或 tmux 窗格中，使用 -w（或 --worktree）标志启动技能 X：
    

claude -w "运行自动化重构脚本并跑通测试"  
  

- 工作流原理：Claude Code 会在项目的 .claude/worktrees/ 下自动创建一个完全独立的 Git 分支和干净的目录。
    
- 完美并行：你在主窗口继续让 Claude 运行技能 Y（修改主分支代码），而后台工作树里另一个拥有独立上下文的 Claude 实例正在疯狂跑技能 X。两个任务互不干扰，完成后台任务后，你只需 git merge 即可。
    

### 4. 自动化级：使用 /loop 周期执行或 /batch 大规模并发

- /loop 周期监听：如果技能 X 是一个需要“每隔一段时间盯一下”的监控任务（例如：每 5 分钟检查一次服务器状态或第三方 API 响应），你可以输入 /loop 5m <运行技能X的指令>。它会以守护进程的形式在后台定期触发，空闲时完全隐形，不干扰你和技能 Y 的交互。
    
- /batch 矩阵加速：如果技能 X 包含大量重复的大规模子任务（例如：把项目里 20 个陈旧的 API 模块全部重构）。直接使用 /batch 命令，Claude 会在后台自动生成 5 到 30 个并行的子代理，利用工作树多线作战。
    

### 5. 掌控力：全局监控与防死循环配置

当技能 X 在后台策马奔腾，你和技能 Y 在前线激战时，你需要掌握绝对控制权：

- 实时看板：终端底部状态栏会实时显示后台任务数。随时输入 /tasks（或部分版本的 /bashes），能直接拉出当前所有后台代理的实时进度条和 Token/资金消耗看板。
    
- Human-in-the-loop (人工审批)：如果后台的技能 X 试图执行高危操作（如修改敏感配置或调用需要付费的外部 MCP 工具），终端会弹出局部覆盖弹窗或通知提醒你进行权限确认，批准后它会继续回后台挂起。
    
- 一键强杀（防无限死循环）：如果后台的技能 X 因为陷入 Bug 发生了自我死循环、疯狂烧 Token，不要慌，在主窗口连按两次快捷键 Ctrl + X 随后按 Ctrl + K，即可瞬间强行杀死当前 Session 下的所有后台 AI 子代理和异步进程，及时止损。
    

💡 专家避坑准则： 如果技能 X 是纯读取/分析任务（如 codebase 审计、日志分析、架构调研），请强烈建议 Claude 开启内建的 Explore 子代理并在后台运行。这样，当它吐出几万字的研究成果时，会先在后台自动执行 /compact（上下文压缩），最终只把几百字的“精炼摘要”返回到你的主线对话中。这能完美防止主对话的上下文因任务 X 的乱入而过快膨胀，帮你省下大笔 Token 费用！

  
**

## 关联笔记

- [[Claude_Code_Skills]] — SKILL.md 格式、技能生命周期管理
- [[Claude_Code_Subagents]] — Fork 子代理隔离机制、Parallel Agents 模式
- [[Claude_Code_Hacks]] — 32 个进阶技巧（含 Worktree、/loop、/batch）
- [[Claude_Code_Hooks]] — Human-in-the-loop 权限拦截、确定性执行层
- [[Human_In_The_Loop]] — HITL 工具调用拦截与高危操作审批