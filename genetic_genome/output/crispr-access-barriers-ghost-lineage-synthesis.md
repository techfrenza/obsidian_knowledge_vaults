---
date: 2026-06-11
source_notes:
  - "[[DSB Repair Species Specificity]]"
  - "[[G-Quadruplex–CRISPR Interference]]"
  - "[[Genomic 3D Structure and CRISPR Efficiency]]"
  - "[[Ghost Lineage Effect on Branch Lengths]]"
  - "[[Mutation Rate vs Fidelity Trade-off]]"
  - "[[Shelterin Complex]]"
tags: [synthesis, crispr-efficiency, dna-repair, ghost-lineage, access-fidelity-filter]
---

# CRISPR Access Barriers & Ghost Lineage Inference Failures — 跨笔记综合

## 综合单元
> 核心笔记：[[DSB Repair Species Specificity]]、[[G-Quadruplex–CRISPR Interference]]、[[Genomic 3D Structure and CRISPR Efficiency]]、[[Ghost Lineage Effect on Branch Lengths]]、[[Mutation Rate vs Fidelity Trade-off]]、[[Shelterin Complex]]
> 邻居笔记：[[Nonhomologous End Joining (NHEJ)]]、[[Homology-Mediated End Joining (HMEJ)]]、[[CRISPR-Cas9]]、[[Noncanonical DNA Structures]]、[[Telomere]]、[[Ghost Lineage]]、[[D-Statistic (ABBA-BABA Test)]]、[[Introgression]]、[[Phylogenetics]]、[[Genetics]]、[[Homology-Directed Repair (HDR)]]

## 一致主线

物理可及性障碍——无论发生在分子尺度（G4 结构、3D 染色质密度）、修复通路选择（NHEJ/HDR/HMEJ 竞争）、物种特异性修复保真度（DSB 缺失/插入偏好），还是进化推断中的谱系采样盲区（幽灵谱系）——系统性地决定精确干预（基因组编辑或基因流推断）的结果，且这些障碍在标准方法论假设下被一贯低估。每一种精确工具——Cas9 切割、D 统计基因流检测——均存在隐性物理或生物约束，以相同方向扭曲其输出：将复杂世界误判为简化的理想模型世界。

## 内在张力

| 观点A | 来源 | 观点B | 来源 |
|-------|------|-------|------|
| 使用远距外群（outgroup）是 D 统计分析的标准最佳实践，可最小化不完全谱系分选（ILS）误差 | [[D-Statistic (ABBA-BABA Test)]] | 使用远距外群实际上**增加**幽灵渐渗误判率——outgroup 距离越远，错误率越高 | [[D-Statistic (ABBA-BABA Test)]]、[[Ghost Lineage Effect on Branch Lengths]] |
| 基因组越大的物种在 DSB 修复中容忍更小的缺失（修复插入填充序列），长期导致基因组膨胀 | [[DSB Repair Species Specificity]] | 大基因组、稳定环境下的物种在突变率-保真度权衡中普遍选择高保真度策略 | [[Mutation Rate vs Fidelity Trade-off]] |
| Shelterin（TRF2）在端粒处主动抑制 NHEJ，防止末端融合 | [[Shelterin Complex]] | NHEJ 是哺乳动物细胞的主导 DSB 修复通路，是 CRISPR 基因敲除的必要机制 | [[Nonhomologous End Joining (NHEJ)]]、[[CRISPR-Cas9]] |

**张力本质**：前两个张力揭示了"标准推荐"操作在特定条件下自我颠覆的反直觉现象（distant outgroup 实为误判风险因子；大基因组选高保真但修复机制本身扩大基因组）；第三个张力揭示了同一细胞内 NHEJ 受到空间限定的双重身份——端粒处的敌人，基因组其他位点的工具。

## 涌现洞察

将六篇核心笔记并置后，浮现出一个单篇视角无法发现的洞察：**基因组编辑的整个技术栈——从 Cas9 分子可及性（G4 结构、3D 密度）、修复通路竞争（NHEJ/HDR/HMEJ），到物种特异性修复保真度，再到进化背景下的突变积累——构成一个层次化、正交叠加的"接入-保真度过滤器"**。Genomic 3D Structure 笔记明确证实了 3D 密度与表观遗传特征的正交性；G4 干扰笔记证实了序列局部障碍的独立性。这意味着针对端粒的 CRISPR 编辑面临的障碍不是加法叠加，而是相互独立的乘法惩罚：高 G4 密度 × 高 3D 压缩 × Shelterin 的 NHEJ 抑制 × 短同源臂可用性下降，同时生效。这一多层正交障碍框架与幽灵谱系的分析平行：两个领域（分子生物学和进化遗传学）中，"标准方法"均建立在简化的单一障碍假设上，而真实生物情境中多重隐性约束的叠加使结果系统性地偏离预测。

## 知识缺口

**当前未回答的关键问题**：多种物理障碍同时作用于同一 CRISPR 靶位点时，效率损失是加法性（X% + Y%）还是乘法性（X% × Y%）？现有笔记仅定性断言 3D 密度与 G4 干扰正交独立，但无定量联合效应模型。

**下一步探索建议**：搜索使用多特征经验数据集（同时包含 G4 稳定性评分、Hi-C 密度、Shelterin 结合位点）对 Cas9 切割效率建模的文献，目标是量化复合物理障碍的交互作用形式。可创建新 wiki 页面 `Combined Physical Barriers to Cas9 Access`，整合 [[G-Quadruplex–CRISPR Interference]]、[[Genomic 3D Structure and CRISPR Efficiency]] 和 [[Shelterin Complex]] 的效应量为统一定量框架，直接指导端粒靶向基因疗法的靶点设计。
