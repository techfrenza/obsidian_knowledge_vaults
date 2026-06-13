---
name: Seed_Tech_CrossModalEvalBatchCadence
description: 多模型互评（cross-modal eval）在每次推理时运行会产生负收益，正确策略是批量化、低频执行
type: seed
concept: Cross-Modal Eval Batch Cadence
hook_insight: 用三个模型互相检查对方的答案听起来是最高可靠性方案，但正确答案是每周跑一次——而不是每次推理都跑；频率越高反而越浪费且收益递减
wiki_link: "[[GBrain_Architecture]]"
---

## 技术核心逻辑

GBrain 的 Book-Mirror Skill 使用三模型互评（Opus 4.7 + GPT-5.5 + DeepSeek）来提升 Skill 的事实准确率。但这带来了一个隐藏的节奏问题：

**每次推理运行互评 vs 每周批量互评**

### 每次推理运行（错误直觉）
- 优点：即时发现错误
- 缺点：
  - 3× API 成本（三个模型并发）
  - 三个模型输出不一致时需要仲裁机制（第四次调用？）
  - 对于已经稳定的 Skill，99% 的互评结果是"通过"，纯粹浪费

### 每周批量运行（GBrain 的实际做法）
- 优点：
  - 成本集中摊销，实际每条知识的验证成本降低 95%+
  - 积累一周数据后，错误模式更明显（单次的随机误差被抹平）
  - Skill 修复后下周再验证，形成自然的迭代节奏
- 缺点：错误存在最长一周的窗口期

## 权衡对比

| 评估频率 | 月度成本 | 错误窗口期 | 信噪比 |
|---------|---------|-----------|------|
| 每次推理 | 3× | 实时 | 低（稳定 Skill 的噪声极多）|
| 每日批量 | 1/30× | ≤24 小时 | 中 |
| 每周批量 | 1/210× | ≤7 天 | 高（足够积累模式）|

## 工程启示

Cross-modal eval 是一种**质量保证（QA）流程**，不是一种**推理增强**手段。与软件工程中的 CI/CD 类比：单元测试在每次提交时跑（轻量），全量集成测试只在发布前跑（重量）。GBrain 的周度互评 = 发布前的重量测试。

参见：[[Skill_Design_Patterns]] 的 Reviewer 模式（Skill 内嵌检查项）；[[Agent_Harness_Engineering]] 的 Evaluator-Optimiser 模式频率选择。
