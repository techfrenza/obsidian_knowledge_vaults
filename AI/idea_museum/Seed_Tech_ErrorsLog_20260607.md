---
title: "Seed_Tech_ErrorsLog_20260607"
tags: [idea_seed, errors-log, agent-memory, anti-intuitive]
type: idea_seed
wiki_link: "[[CLAUDE_md_Best_Practices]]"
created: 2026-06-07
---

**Concept**: ERRORS.md 是比 MEMORY.md 更有杠杆的 AI 记忆机制——专门记录"2次以上才成功的方案"，执行类似任务前先检查。

**Hook Insight**: 大家以为 AI 会"从错误中学习"——实际上 LLM 每次 session 都从零开始，不仅忘记决策，也忘记失败过的方案。ERRORS.md 把失败经验从模型遗忘中抢救出来，强制检索。

**Wiki Link**: [[CLAUDE_md_Best_Practices]]

**Draft Hook**: 你的 AI 一遍又一遍踩同一个坑，因为它没有"失败日志"。ERRORS.md：一个比 MEMORY.md 更有价值的文件——存放"某方案超 2 次才成功"的完整事后复盘。下次遇到类似任务，先检查这个文件。两周后，AI 就不再用你当小白鼠。
