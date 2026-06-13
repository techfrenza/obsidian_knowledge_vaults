# What is Evals in AI Agentic Engineering?

> 查询日期：2026-05-08 | 引用来源：/wiki

---

Evals (Evaluations) 是 AI Agentic 工程中用于持续验证 Agent 行为是否正确的测试框架。它不等同于传统软件测试。

## 核心定义

**Evals 测试的是 harness（环境配置），不是代码逻辑本身。**

传统软件测试：输入 → 函数 → 断言输出（验证确定性逻辑）  
Evals：提示词 + 工具 + 上下文 → 模型 → LLM-as-judge 评分 + CI 门禁（验证环境是否把模型配置成了正确的决策者）

## 三个核心组件

| 组件 | 内容 |
|------|------|
| **Golden Dataset** | 50+ 真实生产案例，覆盖 happy path + 边缘情况 |
| **LLM-as-judge** | 用另一个 LLM 作为评判者，对输出打分（非确定性验证）|
| **CI 门禁** | 每次迭代后自动跑 Regression Set，技术更新不导致业务逻辑倒退 |

## Evals-Driven Development 流程

1. **上线第一天**：建立 50+ 真实案例的 Golden Dataset
2. **每次迭代后**：自动跑 Regression Set（LLM-as-judge）
3. **验证通过标准**：技术更新不导致业务逻辑倒退

## Evaluator-Optimizer 模式（架构实现）

生成者与评估者必须**分离**：

```
Generator（产出）→ Evaluator（专门挑刺）→ 结构化反馈 → Generator（再生成）
```

四阶段循环：**Plan → Execute → Verify → Repair**  
确保每一个变更都经过真实运行的验证。

**为什么分离？** 自评天然有乐观偏差（Skeptical Evaluator 原则）——同一个 Agent 既生成又评估，会系统性地高估输出质量。

## 确定性验证 vs Evals（互补，不替代）

| 类型 | 工具 | 验证什么 |
|------|------|----------|
| 确定性验证 | vitest / tsc / lint / pre-commit hook | 代码语法、类型、格式 |
| Evals | Golden Dataset + LLM-as-judge | Agent 行为是否符合业务期望 |

确定性验证先跑（快、便宜），Evals 后跑（慢、但测行为）。

## 实战模板（任务前定义验收标准）

```
写完后必须通过：
vitest 全绿 + tsc --noEmit + lint +
手动 golden case 对比 + 部署前 staging 验证
```

## 生产监控工具

- **Langfuse**：生产级监控与 Evals，可追踪每次 Agent 调用的输入/输出/评分
- **LangGraph L2 层**：实现自定义 Evaluator + CI 门禁，防止 Agent 跑飞

---

## 引用来源

- `/wiki/Enterprise_AI_Architecture.md`（Evals-Driven Development 节）
- `/wiki/LangGraph_Build_Agents.md`（Evaluator-Optimizer 模式）
- `/wiki/AI_Team_Coding_Practice.md`（确定性验证基础设施）
- `/wiki/Unique_Engineering_Insights.md`（Skeptical Evaluator 原则）
- `/wiki/Agent_Harness_Engineering.md`（Time Scaling 三角色架构）
