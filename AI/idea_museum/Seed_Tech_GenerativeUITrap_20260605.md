**Concept**: Generative UI 让 Agent 可以按需生成用户界面，而不是显示固定页面。三种模式（Controlled/Declarative/Open-ended）代表不同的控制权归属。

**Hook Insight**: 大多数团队在不知情的情况下选择了 "Controlled" 模式（因为框架默认如此），然后在 25 个组件时撞墙，转向 Open-ended（因为 demo 效果好），结果没有任何一个是有意的决策。这两次选择都是"漂移"而非设计。

**Wiki Link**: [[Generative_UI_Architecture]]

**Draft Hook**: 你的 AI 应用在 25 个组件的时候会突然变慢。这不是 bug，这是一个架构陷阱——Controlled GenUI 模式的 token 成本随组件数量线性增长。解法：超过 15 个组件立即切换到 Declarative（A2UI），token 成本从此不再随用例数量上涨。99% 的团队不知道这件事。
