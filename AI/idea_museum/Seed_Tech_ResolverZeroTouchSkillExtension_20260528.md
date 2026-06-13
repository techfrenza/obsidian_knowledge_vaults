---
name: Resolver.md 作为零协调成本的技能路由表
description: GBrain 的 Harness 通过 Resolver.md 路由到任意 Skill，使得新增 Skill 不需要修改 Harness——技能扩展成本为零协调成本
type: seed
concept: 零接触技能扩展（Zero-Touch Skill Extension via Resolver）
hook_insight: 每次你在工具栈里新加一个 Skill，都需要改 Harness 代码来"认识"它——GBrain 的反直觉设计：Harness 永远不知道 Skill 的存在，只有 Resolver.md 知道；新增 100 个 Skill 等于新增 100 行 Markdown，Harness 代码一行未动
wiki_link: "[[GBrain_Fat_Thin_Architecture]]"
---

# Resolver.md 作为零协调成本的技能路由表

## 技术核心逻辑

[[GBrain_Fat_Thin_Architecture]] 架构核心：

```
brain-repo/
├── skills/       ← Fat Skills（100+ 独立 Markdown）
├── resolver.md   ← 唯一需要更新的路由表
└── harness/      ← Thin Harness（只做消息→Resolver→Skill 路由）
```

Harness 工作流：
1. 接收用户消息
2. 读取 Resolver.md
3. 语义匹配触发条件
4. 执行对应 Skill

关键约束：Harness 代码永远不 hardcode 任何 Skill 名称。

与 [[Claude_Code_Skills]] 中 Skill Priority Shadowing 的关系：
- Resolver.md 解决"Harness 如何发现 Skill"
- 但同名 Skill 的优先级问题（Enterprise vs Personal）依然由 Harness 处理
- 零协调成本在单租户场景成立，多租户场景下 namespace 冲突是架构债务

## 优缺点对比

**优势**
- 技能增长不等于代码库增长：100 个 Skill = resolver.md 里 100 行触发条件
- Domain expert（非工程师）可以直接修改 Resolver.md 来注册技能——降低技术门槛
- Git PR 审查 Resolver.md 即等同于审查"系统能做什么的完整目录"

**劣势 / 陷阱**
- Resolver.md 成为单点故障：格式错误或语义描述不清，触发条件失配，Skill 永远不被调用
- 语义路由的正确性依赖模型判断，不可能 100% 确定——需要定期跑触发条件 Eval
- Skill 数量超过 ~200 后，Resolver.md 本身的可维护性成为瓶颈

[Source: wiki/GBrain_Fat_Thin_Architecture]
