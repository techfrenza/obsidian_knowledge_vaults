---
title: "Generative UI Architecture"
parent: "[[AI_Native_Tool_Design]]"
tags: [generative-ui, ag-ui, a2ui, frontend, agent-ui]
category: agent-ui
date: "2026-06-07"
source: "raw/Generative UI Is the New Frontend.md"
---

# Generative UI Architecture

2026 年的前端范式转变：界面不再是设计师画、工程师实现的固定产品，而是由 Agent 在运行时按照用户请求实时生成的动态产物。

[Source: raw/Generative UI Is the New Frontend.md]

---

## 协议栈三层

| 协议 | 职责 | 说明 |
|------|------|------|
| **MCP** | Agent ↔ 工具 | 连接 Agent 到工具服务器 |
| **A2A** | Agent ↔ Agent | 连接多个 Agent 之间的通信 |
| **AG-UI** | Agent ↔ 用户界面 | 状态双向流动的流式层（SSE），承载 tool calls / A2UI schema / MCP App events |

**A2UI**：Google 的规范，Agent 以 schema 形式发出 UI 描述，骑在 AG-UI 上传输。CopilotKit 在生产中实现此规范。

---

## 三种 Generative UI 模式

### 模式一：Controlled（前端控制 UI）

**原理**：预构建组件，Agent 决定渲染哪个。

```typescript
useComponent({
  name: "showExpenseChart",
  description: "Render a breakdown of expenses by category.",
  parameters: expenseChartSchema,
  render: ExpenseChart,
});
```

**特点**：
- 设计系统完全掌控
- 一个 hook，零 Agent 代码修改
- 每个组件 ~400 token 占用上下文

**Token 税问题**：25 个组件 = 每轮 10,000 token 固定成本，且 Agent 可能选错相似组件。

**适用场景**：≤10 个高价值精确流程，设计精确度要求高。

**何时放弃**：组件数量 > 15，工具描述开始语义重叠。

---

### 模式二：Declarative（A2UI，Agent 发出 schema）

**原理**：Agent 发出 JSON schema，前端有 Catalog 将 schema 节点映射到 React 组件。一个工具，支持无限种 UI。

```python
def search_flights(flights: list[Flight]) -> dict:
    return {
        "a2ui_operations": [
            {"type": "create_surface", "surfaceId": ..., "catalogId": ...},
            {"type": "update_components", "surfaceId": ..., "components": FLIGHT_SCHEMA},
            {"type": "update_data_model", "surfaceId": ..., "data": {"flights": flights}},
        ]
    }
```

**Catalog 合约**：Zod schema 定义允许 Agent 发出的组件，Renderer 实现 React 渲染。类型不匹配变成构建错误，不是运行时空白屏。

**Token 经济学**：无论 50 还是 500 种卡片，Agent 只看到一个函数定义。组件库增长时 token 成本保持平坦。

**固定 schema vs 动态 schema**：
- 固定：开发者写组件树，Agent 只填数据
- 动态：次级 LLM 根据对话上下文实时生成组件树

**适用场景**：大量卡片/Widget 类型，关注 token 经济性，长尾用例。

**何时放弃**：需要像素精确的法律/营销页面，布局精度要求极高。

---

### 模式三：Open-ended（无目录，无规则）

**子模式 A：MCP Apps**
MCP 服务器暴露 UI surfaces，Agent 直接控制整个画布（如 Excalidraw）。

**子模式 B：Sandboxed HTML**
Agent 写原始 HTML，前端在沙箱 iframe 中渲染。注意：`sandbox` 属性设为 `allow-scripts allow-forms`，禁止 `allow-same-origin`。

**品牌一致性问题**：样式规则在 Prompt 中可以"引导"但无法"保证"风格。每次可能呈现不同美学。

**适用场景**：一次性查询、临时可视化、用户永远不会重复看到的界面。永远不作为主要界面。

---

## 决策树

```
设计师有像素精确稿？ → Controlled
用例 > 10 个？ → Declarative  
一次性可视化？ → Open-ended
不确定？ → 默认 Declarative，Top 3 流程升级为 Controlled，永远不把 Open-ended 作为默认
```

**诊断方法**：数 render tools 数量。超过 15 个，你已处于 Controlled 模式且接近上限。

---

## 常见失效模式

| 模式 | 失效点 | 修复方向 |
|------|--------|----------|
| Controlled | Agent 选错组件（两个描述语义相似） | 描述改为用户意图而非视觉名称（"当用户要比较整体比例时使用"而非"渲染饼图"）|
| Declarative | 自定义 FlightCard 但总显示通用卡片 | catalogId 字符串在 Agent 侧和 Frontend 侧必须完全一致 |
| Open-ended | iframe 渲染但按钮不触发 | sandbox 标志设置错误（过严或过松被浏览器拒绝）|

---

## 与其他概念的关系

- [[AI_Native_Tool_Design]] — AI-Native 工具设计三大约束，是本页的上层原则
- [[MCP_Connectors]] — MCP 协议层，AG-UI 协议栈的组成部分
- [[Multi_Agent_Architecture]] — Agent 团队架构，GenUI 是其用户界面层
- [[Agent_Context_Architecture]] — 上下文管理，GenUI 的 token 经济依赖上下文优化
- [[Context_Engineering]] — 控制每轮 token 占用的策略
- [[Claude_Code_Skills]] — Hooks 是不消耗上下文的唯一抽象（与 GenUI Controlled 模式 token 税对比）
- [[Anthropic_Agent_SDK]] — AG-UI 是 Claude SDK 的用户界面扩展层
