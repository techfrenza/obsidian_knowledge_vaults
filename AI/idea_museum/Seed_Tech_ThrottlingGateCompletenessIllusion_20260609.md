---
name: Throttling Gate as Completeness Illusion Defense
description: Agent 批处理限流不是成本控制，而是防止"完成假象"导致质量静默崩溃的唯一机制
type: seed
concept: Agent 批处理限流卡点（Throttling Gate as Quality Defense）
hook_insight: 你设置每批最多处理 10 个文件是在省钱——错了，你是在防止 Agent 在第 8 个文件时开始"假装完成"而你永远不知道
wiki_link: "[[Harness_Engineering_Advanced]]"
---

## 技术核心逻辑

Harness Engineering 进阶原则：**无情引入限流卡点**不是成本优化，而是防御性质量设计。

| 场景 | 无限流 | 有限流（最多 N 个）|
|------|--------|-----------------|
| 上下文状态 | 随批次增大，后期截断 | 每批上下文干净，质量均匀 |
| 失败模式 | 静默截断 + "完成假象" | 显式中止 + 递延队列记录 |
| 调试难度 | 无法定位哪个文件触发退化 | 批次边界清晰，二分定位 |

## 深层洞察

"完成假象"是 Agent 批处理中最危险的失效模式：Agent 不报错，不停止，继续输出——但输出质量在上下文接近饱和时已经静默退化。**限流卡点的真正价值是让退化变得可见，而不是保留资源。**

超出部分进入**递延队列（Deferred List）**，不截断不丢失，下次继续。这是一种"显式分段"优于"隐式截断"的工程哲学。

## 优缺点对比

**优势**
- 每批质量稳定可预期
- 失败边界清晰，可调试
- 避免长文本截断导致的部分输出比无输出更危险

**代价**
- 增加批次数量，总运行时间变长
- 需要维护递延队列状态

[Source: wiki/Harness_Engineering_Advanced.md]
