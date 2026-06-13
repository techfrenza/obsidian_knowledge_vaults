---
title: Harness Engineering 进阶指南
parent: "[[Harness_Engineering_Deep_Dive]]"
tags: [harness-engineering, claude-code, context-assets, workflow, production]
category: agent-engineering
stub: false
date: "2026-06-03"
---

# Harness Engineering 进阶指南

从"提示词工程"进化为**"系统治驭工程"（Harness Engineering）**。核心理念：**将模型视为不稳定工程部件**，通过制度化控制平面确保可靠、可重复的工程行为。

## 一、上下文资产与知识治理

### 三层持久化上下文文件
| 文件 | 作用 | 定位 |
|------|------|------|
| `CLAUDE.md` | 架构规则、团队标准、硬性约束 | 行为地图（非说明书）|
| `AGENTS.md` | Agent 如何工作、构建、测试、发现工具包 | 能力地图 |
| `DECISIONS.md` | 架构选择、被拒方案、已知 bug 模式 | 决策日志 |

### 分层加载原则
- 离当前工作目录越近的规则，优先级越高。
- `MEMORY.md` 作为一行指针列表（指向 Topic 文件），入口文件必须保持短小。

### Anti-Rot（防腐化）
- Token 使用量达 60% 时执行 `/compact`。
- **保留错误信息**：失败调用和堆栈追踪帮助模型学习，禁止清除。

## 二、标准化执行流程（Plan-First Workflow）

强制闭环：**Plan → Work → Review → Compound**

### Phase 1：Spec & Plan（人类掌舵）
- 任务前要求 Claude 提出跨仓库分步计划。
- 审查点：尊重现有架构、有检查点和回滚策略。

### Phase 2：Gather-Act-Verify（心跳循环）
- **Gather**：JIT 检索，用 `grep`/`glob` 替代全文件加载。
- **Act**：原子化修改。
- **Verify**：先写 Property-based tests，验证独立于实现阶段。

### Phase 3：Compound（复利沉淀）
任务结束后："总结本会话的决策和教训，更新至 `DECISIONS.md`"。

## 三、技能封装与工具调用

### Skill 插件
- 每个 Skill 含 `SKILL.md`（YAML frontmatter 定义触发条件）。
- 渐进式披露：仅当语义匹配时加载完整指令，节省 Token。

### 高风险工具管理（Bash）
- 受管执行：禁止修改 Git 配置、跳过 Hooks、强制 Push。
- 物理阻挡：不可逆操作（删除、部署）必须经权限审批（allow/deny/ask）。

### MCP 集成
连接企业神经系统（Jira、GitHub、Slack），多 AI 工具共享同一上下文，无需手动复制粘贴。

## 四、并行编排与规模化

### Parallel Agents 工作流
3-10 个独立标签页，每个独立上下文，分别处理不同任务线。

### Sub-agent 分区原则
- **隔离不确定性**：研究任务的局部混乱不污染主线程。
- **强制合成**：主 Agent 必须亲自理解子 Agent 结果后写后续指令，禁止二次外包理解工作。

## 五、错误恢复机制

### 自动恢复层级
- **Prompt Too Long** → 先 Context Collapse → 无效再 Reactive Compact。
- **Output Truncation** → 追加 Meta User Message："直接续写，不要道歉，不要总结"。

### 熔断机制
连续失败 3 次则停止，防止无限循环消耗 API 预算。

## 六、每日高 ROI 行动清单
1. 创建根目录 `CLAUDE.md`，填入已知 AI 错误修复方式。
2. 配置 MCP 连接 Git/GitHub。
3. 每次任务前：定义验收标准（vitest 全绿 + lint 通过）。
4. 每天下班前：低风险重构踢给后台 Background Agents。
5. 每周：审查并精简规则文件，删除过时规则。

## 七、Repo级Harness实现（完整目录结构）

最小可用的Git托管Harness结构：

```bash
.agent/
├── AGENTS.md            # 全局持久化指令（含7-Agent工厂角色定义）
├── skills/              # 可复用Skill库（每个有子目录+skill.md）
├── workspace/           # 实际工作目录
├── tasks/               # TASK-xxx.md 输入（带唯一ID）
├── plans/               # PLAN-xxx.md（人工 /approve 审批后才继续）
├── artifacts/           # 输出物（报告/代码/QA结果）
├── templates/
│   └── QA_Report.md     # 验证清单模板
└── logs/                # 执行日志（每次run追加）
```

**核心原则**：将整个 `.agent/` 目录加入Git版本控制，实现真正可复制的repo级Orchestration。

---

## 八、Agent工程三大防御性设计原则（One-Shot精髓）

来源：生产实践中提炼的工程死铁律（防死循环/Token溢出/工具越权）。

### 1. One-Shot脚本化数据Grounding（解耦原则）
- **错误反例**：让Agent在终端中串行调用 `ls`/`find`/`grep` 扫描整个库（O(N²)命令爆炸）
- **正确实践**：编写独立触发脚本（如 `scripts/diff_raw.py`），一次性吐出干净JSON元数据。Agent只在最上层做高级推理，不做物理遍历
- **收益**：节省高达80%的工具调用延迟，防止上下文窗口炸裂

### 2. 无情引入限流卡点（Throttling Gate）
- 单次Fork会话的上下文空间极其有限
- 每次处理批量上限：**最多10个页面**（2ndbraincompile）、**最多5个笔记**（2ndbrainsyn）
- 超出部分自动记入递延队列（Deferred List），不截断不丢失
- 防止：长文本截断 + "完成假象"导致质量下降

### 3. 优雅退出机制（Passive Defense）
- Skill自动化流不支持高阻断交互式等待
- 参数缺失/冲突/条件不满足时：**输出明确说明 + 追加运行日志 + 优雅终止**
- 禁止陷入无限重试循环

---

## 九、Consult the Council机制

对于高风险架构设计或技术规格，并发调用外部顶级模型API进行交叉盲审：

```python
# 并发调用 ChatGPT/Gemini Pro/Grok 对当前技术Spec进行无情挑刺
# 几美分的API开销 → 换取多角度严苛审查
# 合并结果 → 单份交叉审查报告 → Claude执行修复
```

**应用场景**：关键 Skill 发布前 / 架构设计定稿前 / 生产Harness规则更新前。

---

## 关联

- [[Harness_Engineering_Deep_Dive]] - Harness 核心概念
- [[Agent_Harness_Engineering]] - Agent Harness 工程
- [[Skill_Engineering_10_Rules]] - Skill 工程规则
- [[CLAUDE_md_Best_Practices]] - CLAUDE.md 最佳写法
- [[Claude_Code_Skills]] - Skills 生态
- [[Context_Engineering]] - 上下文工程
- [[Seven_Agent_Software_Factory]] - 7-Agent工厂：与Harness目录结构配合使用

[Source: raw/Claude Code Harness Engineering 指南.md, raw/Agent Harness 框架的完整 repo 级实现方案.md, raw/Chat from Grok and Gemini (2).md]
