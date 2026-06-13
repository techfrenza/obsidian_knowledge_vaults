---
name: 写操作代理禁止降级原则
description: LLM 故障时写操作代理宁愿整体失败也不降级到低质量模型，因为错误数据比无响应危害更大
type: seed
concept: Write Agent 禁止 Capable→Fast 降级
hook_insight: "你的 AI 代理设计了'优雅降级'——主模型挂了自动切换小模型。恭喜，你刚刚让它可以向财务系统写入错误数据。写操作代理唯一安全的降级是：直接报错，不是换个模型继续跑"
wiki_link: "[[SAP_Agent_Resilience]]"
---

# 写操作代理禁止 Capable→Fast 降级

## 技术核心逻辑

`capable → fast` 降级在**读操作**场景完全合理：检索信息、生成摘要，即使质量稍差也不会造成不可逆损害。

但在**写操作**场景（财务数据写入、数据库更新、合同生成），这个逻辑完全反转：

- Haiku/Flash 等快速层模型的输出质量不足以支撑写操作的语义精度要求
- 一旦错误数据写入，回滚成本远高于"服务中断"
- 因此 `fallbacks=[]`：所有 capable-tier 模型全部失败时，代理**必须以失败终止**

```python
# 写操作代理的 LiteLLM Router 配置
router = Router(
    model_list=[...capable models...],
    fallbacks=[],  # 写代理：禁止 capable→fast 降级
)
```

## 优缺点对比

| 维度 | 禁止降级（写操作） | 允许降级（读操作） |
|------|------|------|
| 数据安全 | 高：错误只有"无响应"，没有"错误数据" | 可接受：读结果质量降低但无副作用 |
| 可用性 | 低：提供商全线故障时服务中断 | 高：至少有结果返回 |
| 运维负担 | 高：必须保证 cross-provider capable 覆盖 | 低：随意降级即可 |

## 关键洞见

"优雅降级"是系统韧性的默认直觉，但**写副作用操作的正确设计是"清晰失败，拒绝错误执行"**。这与 Rule 12（Fail visibly, not silently）一脉相承。

[Source: wiki/SAP_Agent_Resilience.md]
