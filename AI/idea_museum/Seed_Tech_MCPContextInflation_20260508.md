---
type: seed
source: wiki_scan
date: 2026-05-08
---

# MCP Context Inflation Trap

**核心逻辑**：MCP 是官方推荐的工具集成标准，但它的实现代价是将**所有**已注册工具的定义一次性加载进上下文窗口——即使当前任务只需要其中一个工具。

**权衡对比**：

| 方式 | 优点 | 隐性成本 |
|------|------|----------|
| MCP Server | 自动发现工具、统一协议 | 每次调用加载全部工具定义 → 上下文膨胀 |
| 硬编码 API endpoint | 零上下文开销 | 无自动发现，需手动维护 |

**反直觉点**：MCP 被设计为减少集成复杂度，但在长会话或工具集庞大的场景下，它反而是**主要的上下文污染来源之一**。正确策略：只在工具集 ≥ 3 且频繁切换时用 MCP；单一固定端点直接硬编码。

**Wiki Link**: [[MCP_Integration_Playbook]], [[MCP_Production_Decision_Framework]], [[Claude_Code_Hacks]] (Hack #24), [[Context_Engineering]]
