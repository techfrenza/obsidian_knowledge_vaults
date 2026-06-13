---
title: Claude 优化（8 大实战修复）
aliases: ["Claude 8修复", "Claude 优化", "Claude 输出质量"]
tags: [claude, optimization, quality, tools, models]
category: prompt-engineering
parent: "[[Claude_Code_Advanced_Features]]"
created: 2026-05-15
date: "2026-05-15"
---

# Claude 优化（8 大实战修复）

Parent: [[Claude_Code_Advanced_Features]]
Source: [Source: raw/Claude 优化.md]

## 问题定位
Claude 输出质量下降的根本原因：**工具选错、模型选错、上下文未管理、提示词结构差**。

## 8 大修复（按优先级）

### 修复 #8：选择正确工具
| 场景 | 工具 |
|------|------|
| 日常简单任务 | Claude Chat（快速问答、单次 prompt） |
| 本地文件夹/自动化 | Claude Cowork（项目文件夹 + 本地记忆） |
| 构建代码/仪表盘/脚本 | Claude Code（专为创建 + 托管设计） |
| 所有长期项目 | 先建 Claude Projects（加载 Instructions、目标、偏好） |

### 修复 #7：模型选择
- 复杂 Agent、架构设计、RAG pipeline → **Opus（思考模式）**
- 快速迭代/简单代码 → **Sonnet**
- 日常聊天 → 保持默认

### 修复 #6：Master System Prompt（项目级）
```
你是顶级 AI 应用开发专家。
- 永远先独立思考再输出
- 不确定时明确说 "I don't know"
- 每次输出必须包含可直接复制的代码块、XML 结构、edge case 处理
- 记住我所有重要偏好并存入 memory
- 思考深度：每步考虑可扩展性、生产部署、成本优化
```

### 修复 #5：正确 Prompting 流程
1. 先给足 context（Voice Message 脑暴更快，推荐 WhisprFlow 插件）
2. 明确要求 AI 先问 10-50 个澄清问题再执行
3. 指定输出格式（"像附上的示例一样结构化，用 XML 标签"）

### 修复 #4：强制使用 XML Tags
Anthropic 官方推荐，速度 + 准确率显著提升：
```xml
<context>你的项目背景</context>
<task>具体要求</task>
<output_format>必须包含代码 + 测试用例 + 部署步骤</output_format>
```
懒人方法：让 Claude 自动生成适合当前任务的 XML tags。

### 修复 #3：开启 Claude Tools + Connectors
- **Research mode + Web Search**：实时查最新文档、API，避免幻觉
- **Files 上传**：代码仓库、设计文档
- **Connectors**：Notion、Drive、GitHub，详见 [[MCP_Connectors]]

### 修复 #2：保持 Fresh Context（避免 Context Rot）
- 规则 1：每 2 周清理 Project 文件，删除 3 周未引用内容
- 规则 2：每周 Review Memory，删掉过时内容
- 规则 3：聊天变乱就新建 chat，让 Project Memory 接管

### 修复 #1：Master Workflow（最高阶组合）
将以上 7 条全部组合：
> 选对工具 → Master System Prompt → XML + 正确 Prompt → 开 Tools → 每次新 chat 保持 fresh context

## 立即落地 Checklist
1. 创建/更新主 Project + 粘贴 Master System Prompt
2. 切换开发任务到 Claude Code + Opus
3. 下个 prompt 强制加 XML tags + 先问澄清问题
4. 开启 Web Search + 清理一次 Memory
5. 每周日固定 15 分钟 Project 健康检查

## 关联概念
- [[CLAUDE_md_Best_Practices]] — Master System Prompt 的持久化存储
- [[Context_Engineering]] — Fresh Context 与 Context Rot 的技术细节
- [[MCP_Connectors]] — Tools + Connectors 的配置
- [[Claude_Code_Advanced_Features]] — 完整高级功能体系
- [[Claude_Code_Product_Positioning]] — Claude 产品矩阵定位
- [[Agent_Engineer_Roadmap]] — 8 大修复是 Phase 1–2 的操作手册（模型选择/Prompt结构/上下文管理）

- [[Claude_Code_MOC]] — Claude Code 生态系统学习地图