---
type: seed
source: wiki_scan
date: 2026-05-08
---

# Deny List Always Wins: Why AI Safety Is an Exclusion Game

**X Hook 草稿**：

> Everyone focuses on what to ALLOW Claude to do.
> 
> The pros focus on what to DENY.
> 
> Here's why: the "allow" list grows indefinitely.
> The "deny" list is finite and absolute.
> 
> In Claude Code permissions: deny always overrides allow.
> That's not a limitation. That's the architecture.
> 
> Build your safety layer as a deny list, not a trust list.
> Exclusion is more reliable than inclusion — in AI and in life.

**反直觉核心**：安全设计的直觉是"定义什么是被允许的"（白名单）。但 Claude Code 的权限模型揭示了更深的真相：在高速自主代理系统中，**允许列表永远是不完整的**（因为你无法预见所有合法用例），而拒绝列表可以做到完整（你明确知道什么是不能发生的）。Deny wins 不是设计限制，是安全第一性原理。

**灵感来源**: [[Claude_Code_Hacks]]（Hack #30 deny list always wins）, [[Claude_Code_Security]], [[Claude_Code_Settings]], [[Human_In_The_Loop]]
