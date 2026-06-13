---
type: seed
source: wiki_scan
date: 2026-05-06
---

# Seed: 你的 AI Agent 越来越聪明，但你的系统正在变笨

**[Hook Insight]**

每个人都在讨论让 Agent 更自主。
没有人在讨论：**当 Agent 自主决定不调用某个 Skill 时，你会知道吗？**

Hooks 是事件驱动的——无论 Agent 想不想，文件保存后 lint 一定跑。
Skills 是请求驱动的——Agent 认为不需要时，根本不会触发。

大多数团队把规范写进 Skills，以为万无一失。
然后某天发现代码库里有一半文件格式不对。
Agent 没有出错——它只是认为那次"不需要"格式化。

**真正的系统可靠性 = 确定性执行比例。**
一个 80% 靠 Skills + 20% 靠 Hooks 的团队，比 20% Skills + 80% Hooks 的团队多 4 倍的"Agent 判断失误"暴露面。

结论：你加的每一条"让 Agent 去做"的规则，实际上是在减少系统的确定性。
规范越重要，越应该放进 Hook，而不是 Skill。

**[Wiki Link]** [[Claude_Code_Hooks]] — 确定性执行层 vs 请求驱动层核心区分  
**[Wiki Link]** [[Claude_Code_Skills]] — Skill 六大模式与适用边界
