---
type: seed
source: wiki_scan
date: 2026-05-02
concept: "Subagent 上下文隔离的真实成本收益"
wiki_link: "[[Claude_Code_Subagents]]"
---

# [Concept] Context Isolation as a Write-Once Architecture

Subagent 隔离的核心不是"省 token"，而是**防止探索噪声反向污染主线决策**。  
一次扫描任务如果在主线程跑，会向主 Agent 注入 10k+ tokens 的模糊信息（文件路径、依赖树、过期注释），让后续所有决策都带上这层噪声。Subagent 的摘要机制（≤2000 token condensed summary）强制做了一次有损压缩——但这个"损"是有意义的信息筛选。

## 权衡核心

| 选择 | 优势 | 隐藏代价 |
|------|------|---------|
| 主线程执行 | 上下文完整 | 探索噪声永久残留 |
| Subagent 隔离 | 主线程干净 | 摘要可能丢失关键细节 |
| Fork 继承 | 继承已有理解 + 10x 成本优势 | 需要预判哪个子任务值得 fork |

## 非直觉点

Fork 后 prompt cache 变得"便宜 10 倍"——这意味着**越晚派生 Subagent（等主线程积累了更多 codebase 理解），Fork 的单位价值越高**。早派生 = 浪费缓存优势。

*[Source: [[Claude_Code_Subagents]], [[Agent_Harness_Engineering]]]*
