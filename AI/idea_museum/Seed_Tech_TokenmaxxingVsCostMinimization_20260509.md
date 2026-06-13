---
name: Tokenmaxxing vs Cost Minimization Trade-off
description: 反直觉：最大化 Token 投入（而非最小化）才能最大化 Agent 输出质量——Token 是买可靠性的货币，不是待优化的成本
type: seed
---

# Tokenmaxxing vs Cost Minimization

[Concept] **Token 是可靠性的货币，不是待削减的成本**

[Trade-off]

| 策略 | 优 | 劣 |
|------|----|----|
| **省 Token（主流）** | 成本低、速度快 | Agent 迷路、跑偏、输出质量不稳定 |
| **Tokenmaxxing（反主流）** | 质量稳、执行力强、错误率低 | 单次成本高 |

关键洞见：省 Token 省的是钱，但付出的代价是工程师的时间（Debug + 返工）。Garry Tan 实证：13 年不写代码，靠 Tokenmaxxing 数月产出数十万行代码。**返工成本 >> Token 成本。**

[Hook Insight] 你以为在优化成本，其实在制造技术债。每一个被省掉的 Context Token，都是一次 Agent 不得不猜测的机会。

[Wiki Link] [[Tokenmaxxing]] · [[Context_Engineering]] · [[Harness_Engineering_Deep_Dive]]
