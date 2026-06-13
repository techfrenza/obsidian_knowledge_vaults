---
title: Karpathy 方法论
parent: "[[Agent_Engineer_Mental_Models]]"
tags: [karpathy, claude-md, behavior-governance, karpathy-loop, llm-wiki]
category: prompt-engineering
stub: false
date: "2026-06-03"
---

# Karpathy 方法论

Andrej Karpathy 提出的 AI 工程实践体系，核心包含：CLAUDE.md 行为治理、Karpathy Loop 迭代优化、LLM Wiki 知识架构。

## 一、CLAUDE.md 行为治理

### Karpathy 4 Rules（2026 年 1 月，错误率从 41% → 11%）

| 规则 | 内容 |
|------|------|
| **Rule 1：Think Before Coding** | 明确陈述假设，呈现权衡取舍，先澄清再猜测 |
| **Rule 2：Simplicity First** | 只写解决问题所需最少代码，禁止推测性功能 |
| **Rule 3：Surgical Changes** | 只改必须改的代码，匹配现有代码库风格 |
| **Rule 4：Goal-Driven Execution** | 给成功标准，而非步骤；循环迭代直到满足 |

### 2026 年 5 月扩展版（+8 条 Agent 编排规则）

| 规则 | 内容 |
|------|------|
| **Rule 5：Don't make the model do non-language work** | 确定性逻辑（重试、路由、升级）写在代码里，不让模型决定 |
| **Rule 6：Hard token budgets, no exceptions** | 每个任务设 Token 上限，防止死循环耗尽上下文 |
| **Rule 7：Surface conflicts, don't average them** | 代码库中的矛盾模式必须明确指出，禁止折中 |
| **Rule 8：Read before you write** | 修改前必须理解相邻代码，防止冲突函数出现 |
| **Rule 9：Tests are not optional, but they're not the goal** | 禁止为通过测试而返回常数，测试必须有意义 |
| **Rule 10：Long-running operations need checkpoints** | 多文件重构必须设阶段性检查点 |
| **Rule 11：Convention beats novelty** | 即使有"更好"模式，也必须遵循项目既定规范 |
| **Rule 12：Fail visibly, not silently** | 任何跳过或异常必须显式输出，禁止静默失败 |

### CLAUDE.md 合规上限
> 超过 **200 行**时，Claude 遵守度急剧下降，重要规则被噪声淹没。

## 二、Karpathy Loop（自主迭代优化）

**结构**：一个目标 + 一个 Agent 可修改文件 + 一个评分工具 + 循环（丢弃失败版本，保留改进版本）

**验证成果**：Shopify 将此模式应用于 Liquid 引擎，一夜实现渲染速度 +53%、内存分配 -61%。

**现代应用**：让 Agent 在用户睡眠时科学地测试并重写自己的 SOP。

## 三、LLM Wiki 知识架构

用简单人类可读 Markdown 文件夹替代复杂 RAG 基础设施。

**结构**：
- `raw/` - 原始材料
- `wiki/` - LLM 生成的知识页面
- `index/` - 交叉引用
- `log/` - 操作历史

**效率**：Token 使用量降低 **95%**，因为 LLM 自己维护摘要和索引。

**演化机制**：摄入新信息时，Claude 自动更新 10–15 个相关页面并建立反向链接，形成不断进化的第二大脑。

## 四、AI Operating System（AIOS）框架

**四个 C（按顺序构建）**：
1. **Context**：业务/语音数据
2. **Connections**：工具/API 接入
3. **Capabilities**：可复用技能
4. **Cadence**：自主例行程序

**默认转变**：开始任何任务前先问"AI 能完成这 30% 的工作吗？"

## 五、MCP 原语（AI 的 USB-C）

| 原语 | 控制方 | 用途 |
|------|--------|------|
| **Tools** | 模型控制 | 执行计算、运行脚本 |
| **Resources** | 应用控制 | 被动数据源（Google Drive、本地文件）|
| **Prompts** | 用户控制 | 预构建模板、slash commands |

## 六、Skills 作为杠杆化 SOP

- 渐进式上下文加载：初始仅扫描名称和描述，语义匹配后才加载完整指令。
- 可包含 on-demand hooks：在高风险技能激活时阻挡危险操作（如 `rm -rf`）。

## 矛盾与争议

- Karpathy 主张 CLAUDE.md < 200 行，但实践中复杂项目往往需要更多规则。建议通过分层文件（`DECISIONS.md`、子目录 `CLAUDE.md`）解决。

## 关联

- [[CLAUDE_md_Best_Practices]] - CLAUDE.md 最佳写法
- [[Agent_Engineer_Mental_Models]] - Agent Engineer 心智模型
- [[Harness_Engineering_Advanced]] - Harness 进阶指南
- [[Context_Engineering]] - 上下文工程
- [[Claude_Code_Self_Evolving]] - Claude Code 自我进化

[Source: raw/Karpathy methodology.md]
