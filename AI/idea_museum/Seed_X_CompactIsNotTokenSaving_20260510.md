---
name: Seed_X_CompactIsNotTokenSaving
description: /compact 命令的真正价值是认知卫生而非成本节约——60% 触发点是注意力管理的时机判断
type: seed
concept: /compact as Attention Hygiene
hook_insight: 你以为 /compact 是为了省 Token，其实它是注意力重置——当 Claude 开始问"你想做什么"，它不是在偷懒，是在告诉你上下文已经腐化了
wiki_link: "[[Tokenmaxxing]]"
---

## X Hook 草稿

**Hook 1（认知框架型）：**
> 大多数人在 Claude context 快满时才用 /compact。
>
> 正确触发时机：60% 满时，不是 90% 满。
>
> 原因：60-90% 的 context 是注意力最差的区间。
> 模型已经被淹没了，只是还能勉强回答。
> /compact 不是省钱，是把注意力拉回来。

**Hook 2（信号识别型）：**
> Claude 开始问"你想要我做什么？"
>
> 这不是它在偷懒。
> 这是 context rot 的早期信号——关键决策已经"漂移"到中间段，被后来的内容稀释了。
>
> 这时候的正确反应：立即 /compact，不是继续对话。

**Hook 3（实践操作型）：**
> 我在 CLAUDE.md 里写了这条规则：
>
> "Compact 时保留：架构决策、未解决 issue、Compact Instructions 本身。"
>
> 它把 /compact 从"会丢东西的操作"变成了"有选择的重聚焦"。
>
> 上下文管理不是技巧，是工程纪律。
