---
type: seed
source: wiki_scan
date: 2026-05-06
---

# Seed: 你雇了 AI Agent，结果它变成了你最贵的抄写员

**[Hook Insight]**

用了 3 个月 AI Agent，团队速度快了，但有人开始说"好像代码质量没变"。

原因：你把 Agent 的 Compound 步骤省掉了。

[[AI_Team_Coding_Practice]] 的核心洞见：
```
Plan → Work → Review → Compound
```

80% 的团队做了前三步，跳过第四步。

Compound = 把这次任务的决策、被拒绝的方案、踩过的 bug 写回 DECISIONS.md。

跳过 Compound，你得到的是：
- 下一个 session 里 Agent 会犯同一个错误
- 三个月后的 Agent 和第一天一样"没有记忆"
- 你的团队知识还是全在人脑里，只不过现在人脑更懒了

没有 Compound，AI Agent 就是一个高速打字员。
有了 Compound，它才是一个真正在学习你的系统的工程师。

**反直觉结论**：AI Agent 的复利不来自模型升级，来自你愿不愿意在每次任务结束后花 2 分钟写 DECISIONS.md。

**[Wiki Link]** [[AI_Team_Coding_Practice]] — Plan→Compound 四步闭环  
**[Wiki Link]** [[Claude_Code_Self_Evolving]] — Corrections→Rules 自动进化循环
