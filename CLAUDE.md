# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# LLM Wiki 共享核心规则 (Shared Core — All Vaults)

> 本文件包含所有 Obsidian wiki 库通用的共享规则（§3–§10）。
> 各子库的 CLAUDE.md 仅含库专属配置（§1–§2a），共享规则由 Claude Code 自动从此父目录加载。

**所有回答都必须用中文回答。**

---

## 1. 仓库结构速览

本仓库为多 Vault 的 Obsidian 知识库，根目录下有 18 个 vault 子目录。每个 vault 结构相同：

```
<vault>/
  raw/          # 只读原始素材（markdown、PDF）
  wiki/         # 原子化知识实体（读写）
  idea_museum/  # X 平台灵感种子（读写）
  output/       # 查询结果、综合报告、草稿（读写）
  _history/
    decisions.md       # 处理逻辑偏好 + 已处理来源（不记录运行结果）
    runs.md            # 技能执行日志（Health Score、新增文件）
    synthesized_notes.md  # Synthesis 已覆盖笔记列表
  CLAUDE.md     # 本 vault 专属配置（§1–§2a），共享规则从父目录加载
```

根目录 `CLAUDE.md`（本文件）= 共享核心规则。各 vault 的 `CLAUDE.md` 仅覆盖 §1–§2a（角色权限 + 领域分类标签），结构完全一致。

## 2. 技能系统（Skills）

所有知识库操作通过 Claude Code Skills 触发，**不是**直接的命令行工具：

| 技能指令 | 作用 |
|---|---|
| `/2ndbraincompile` | raw → 原子化 wiki → 链接增强，自动生成 idea seeds |
| `/2ndbrainidea` | 深挖 wiki → idea_museum（独立于 Compile 的周期性挖掘） |
| `/2ndbrainsyn` | 跨笔记综合 → output/（自动轮转或显式指定笔记） |
| `/2ndbrainlint` | 修复孤岛、重复定义、弱连接 |
| `/2ndbrainwrite` | 基于 wiki 生成长篇文章或报告 → output/ |
| `@2ndbrain-pipeline` | 后台 agent：Compile → Idea → Synthesis 全流程 |

**跨 vault 工作时**：先确认当前在哪个 vault 子目录下，再运行技能；技能读写的路径均相对于该 vault 根目录。

---

## 3. Compile 规则

### A. 去重与合并
- `diff` 思维：已有 wiki 页面时，合并信息而非新建重复页。
- 检查 `_history/decisions.md` 中已处理来源，跳过重复 raw 文件。

### B. 原子化约束
- 每个 wiki 页面 = 一个概念。禁止在单页面塞多个实体。
- 必须带来源：`[Source: raw/filename.md]`。

### C. YAML Frontmatter（必填）
```yaml
---
title: "概念名称"
parent: "[[父节点]]"
tags: [tag1, tag2]
source: "raw/filename.md"
category: "领域分类"
date: "YYYY-MM-DD"
stub: false
---
```
- `parent` 必须引用现有 wiki 节点（YAML 值必须加引号）。
- `category` 从各库 §2a 的 Vault 领域分类标签中选取。
- `date` 为创建日期（ISO 格式）。
- `stub: true` 标记内容不完整的占位页；完成后改为 `false`。
- 出站链接目标 ≥ 3（parent 计入），最低 ≥ 2。

### D. 链接增强（保存前执行）
1. `ls /wiki` 获取所有实体名。
2. 自动补全正文中的 `[[实体名]]` 引用。
3. 确认双向连通：A 链接 B → 检查 B 是否需反链 A。

---

## 4. Idea Museum 规则

- **触发**: Compile 中发现反直觉 / 违反常识 / 高传播潜力信息 → 自动创建 Seed。
- **文件命名**: `Seed_{Tech|Social|X}_{ConceptName}_{YYYYMMDD}.md`
- **YAML Frontmatter**（必填，禁用 H1 标题行）:
  ```yaml
  ---
  name: "Seed 名称"
  description: "一句话摘要"
  type: seed
  concept: "核心概念定义"
  hook_insight: "反直觉洞察"
  wiki_link: "[[相关 Wiki 页面]]"
  ---
  ```
- **正文结构**（对比/推理类用表格，其余用列表）:
  - `## 技术核心逻辑` — 理论论证 + 可选对比表格
  - `## 关联` — `[[wiki links]]` 列表（每条加一行描述）
  - `## Hooks 草稿（X平台）` — 仅 X-type seed 必须包含；X 平台首句草稿（疑问/颠覆/数据开头）
- **格式禁令**: 正文中禁用 `[...]` 方括号（Obsidian 解析为任务列表）；`wiki_link` 字段使用 `"[[...]]"` 带引号形式。

### IdeaHarvest 去重协议

每次运行 `/2ndbrainidea` 前：
1. `ls output/IdeaHarvest_*.md` 获取所有历史收割文件。
2. 读取最新一份，提取已记录的洞察关键词/主张列表。
3. 本次收割**跳过**与历史收割内容实质相同的洞察（即使措辞不同）。
4. 在新 IdeaHarvest 文件头部注明：去重基准文件名 + 排除洞察数量。
5. 文件命名：`IdeaHarvest_YYYY-MM-DD.md`（同日多次则追加 `_HHMM`）。

---

## 5. 查询与输出规则

- **必须先 grep**: 任何知识问题，先 `grep -r "关键词" wiki/` 再回答。
- **只基于 wiki 事实**: 信息不足时明确说明并建议查阅 `/raw`。
- **置信度标注**: `高`（wiki 直接陈述）/ `中`（交叉推断）/ `低`（弱证据）。
- **保存条件**: 仅用户明确知识查询时保存 → `/output/Question_YYYY-MM-DD_HHmm.md`，末尾注明引用路径。

### 查询输出文件格式（`Question_YYYY-MM-DD_HHmm.md`）

```markdown
---
date: YYYY-MM-DD
query: "用户原始问题"
tags: [query, <主题标签>]
---

# <问题标题>

## 回答
<基于 wiki 事实的回答，每条主张附置信度>

## 引用来源
- [[wiki页面1]] — 引用理由
- [[wiki页面2]] — 引用理由

## 知识缺口（如有）
<wiki 中未覆盖、需进一步探索的方向>
```

---

## 6. 写作风格

- **Karpathy 密度**: 每句话承载一个完整信息单元，无冗余。
- **格式优先级**: `表格 > 列表 > 散文`（对比类/架构类优先表格）。
- **引用闭环**: 所有回答和笔记必须互联，孤立陈述是草稿不是知识。
- **认知层次**: `合成 > 关联 > 摘要`。

---

## 7. 证据质量分级（Evidence Tier）

所有 wiki 笔记和综合报告中，每条主张必须标注信源层级。Compile 生成新笔记时在"矛盾与争议"章节注明各来源层级；Synthesis 输出的张力表中在"来源"列附加层级标记。

| 层级 | 标记 | 说明 |
|------|------|------|
| A | `[A]` | 同行评审论文，可被独立复现实验证伪 |
| B | `[B]` | 政府解密文件、机构报告或专家宣誓证词，内容可查但可能经过筛选 |
| C | `[C]` | 可信研究者/内部人士的直接证词或正式转述，无独立物证 |
| D | `[D]` | 主观体验者叙事，高度个体化，文化采样痕迹明显 |
| E | `[E]` | 匿名来源或无法溯源，自我引用循环风险高 |

**警告规则**：若 A 级节点被用于支撑 D/E 级主张的可信度，在综合文档中必须显式标注"认识论互保风险"，不得隐性引用。

---

## 8. 质量控制

- **矛盾不覆盖**: 发现矛盾 → 添加 `## 矛盾与争议` 章节，记录双方观点，不选边。反驳方论点须标注最强具体论据（作者/来源/计算，如 Tegmark 退相干时标计算、publication bias meta-analysis），不得仅写"有人反对"。
- **Health Check**（每次 Compile 后记录到 `runs.md`）:
  - 总 wiki 页面数 / 有 parent 比例 / 出站链接 ≥ 3 比例 / 有来源标注比例 / 孤岛数（目标 0） / 新增 Seeds 数

---

## 9. 记忆与自进化

- `_history/decisions.md`: 处理逻辑偏好 + 已处理来源（不记录运行结果）。
- `_history/runs.md`: 技能执行结果（Health Score、新增文件、变更摘要）。
- `_history/synthesized_notes.md`: Synthesis 已覆盖笔记列表，供 `/2ndbrainsyn` 轮转去重使用。
- `_history/Linting_Report_YYYY-MM-DD_HHmm.md`: 每次 Lint 运行的详细报告。

---

## 10. 禁止行为

| 禁止 | 原因 |
|------|------|
| 重复创建已有 wiki 页面 | 路由歧义，破坏链接唯一性 |
| 创建无 parent 的孤岛页面 | 复利飞轮断裂 |
| 不 grep 直接基于模型记忆回答 | 引入幻觉 |
| 私自覆盖矛盾信息 | 知识损毁，不可逆 |
| 在 idea_museum 使用 `[...]` 方括号 | Obsidian 解析为任务列表 |
| 将技能执行结果写入 decisions.md | 两份日志职责分离 |
| 单页面塞入多个概念实体 | 违反原子化，链接粒度失控 |
| 遗留 `_tmp_*.py` / `_tmp_*.json` 根目录文件 | 污染 vault 根目录；每次技能运行结束后删除临时脚本 |
| 修改 `wiki/index.md` 的结构节点定义 | index.md 是全局枢纽；内容变更须经用户确认 |
| `矛盾与争议` 章节仅写"有人反对"而不引用具体论据 | 无作者/来源的反驳是空洞声明，等同于遮蔽真实张力 |
