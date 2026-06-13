---
name: Cloudflare 2工具覆盖2500接口模式
description: 只暴露 search + execute 两个薄工具，让 Agent 自己写脚本覆盖 2500+ endpoints，上下文减少 85%+
type: seed
concept: 薄工具接口 + Agent 自写脚本（Cloudflare MCP 模式）
hook_insight: "MCP 最佳实践告诉你把每个 API endpoint 都包装成工具——Cloudflare 的做法是只给两个工具（search + execute），让 Agent 自己写脚本覆盖 2500+ 接口。工具越少不等于能力越少，它等于上下文消耗少 85%"
wiki_link: "[[MCP_Production_Decision_Framework]]"
---

# 薄工具接口 + Agent 自写脚本（Cloudflare MCP 模式）

## 技术核心逻辑

**常见错误设计**：把每个 API endpoint 都映射为一个 MCP Tool。
- 2500 个 endpoints → 2500 个 tool definitions
- 每次 Agent 调用前，所有工具 schema 都注入上下文
- 上下文膨胀 → lost in the middle → 性能下降

**Cloudflare 的反直觉方案**：
```
只暴露 2 个薄工具：
  search(query) → 找到相关 API 文档
  execute(code) → 执行 Agent 自己写的脚本

Agent 收到 search 结果后，自己写代码调用任何 endpoint。
```

效果：
- 上下文 ~1K tokens（vs 数万 tokens）
- 覆盖能力：2500+ endpoints（没有减少）
- 关键：**工具数量和能力覆盖是两个独立变量**

## 优缺点对比

| 维度 | 薄工具（2个）| 厚工具（N个）|
|------|------|------|
| 上下文消耗 | 极低（~1K tokens）| 极高（随工具数线性增长）|
| Agent 自由度 | 高（自写脚本，任意组合）| 低（只能用预定义工具）|
| 安全审计 | 复杂（Agent 代码需沙箱）| 简单（每个工具行为已知）|
| 维护成本 | 低（不需要为每个 endpoint 写 schema）| 高（endpoint 变化需更新 schema）|

## 关键洞见

这是"能力通过语言表达，而非通过接口枚举"的极端案例。Agent 不需要每个操作都有一个专属工具——它需要的是**理解能力和执行能力的组合**。

[Source: wiki/MCP_Production_Decision_Framework.md]
