---
type: seed
source: wiki_scan
date: 2026-05-05
---

# Seed: 异步杠杆 — 睡眠时间是最被低估的计算资源

**[Concept]** Night Queue 系统将人类的非工作时间转化为 Agent 的生产时间，实现"8 小时睡眠 = 8 小时并行开发"。

**[Hook Insight]** 传统工作流：你工作 → Agent 配合你。Night Queue 颠覆：你下班前 10 分钟写一个 Markdown 任务清单（3-5 个任务），Background Agents 整夜执行，你早上 review PR。关键不是技术，是**思维转变**：把 Agent 当异步员工而非同步工具。权衡在于任务必须具有"自包含上下文"——任务描述模糊导致 Agent 夜里绕圈、产出垃圾。成功的 Night Queue 需要你在任务描述时就完成了 80% 的思考工作。

**[Tech Trade-off]**
- 异步 Agent：时间杠杆最大化 → 缺点：调试窗口滞后 8 小时，错误链可能扩大
- 同步交互：即时反馈、快速纠偏 → 缺点：人类成为瓶颈，无法并行

**[Wiki Link]** [[AI_Orchestration_System]] → [[Claude_Code_Routines]]

*[Source: wiki/AI_Orchestration_System.md | 2026-05-05]*
