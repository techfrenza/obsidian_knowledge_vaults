---
title: "iPSC Reprogramming"
parent: "[[Biological Age Reversal]]"
tags: [iPSC, reprogramming, epigenetics, CRISPR, cellular-rejuvenation, OSKM, aging]
source: "raw/Decoding Aging through iPSC Reprogramming Advances and Challenges.md"
category: "长寿研究"
date: "2026-07-11"
---

# iPSC Reprogramming

诱导多能干细胞（iPSC）重编程——通过短暂表达山中因子（OSKM）恢复细胞多能性并逆转衰老标志物的技术。**部分重编程**（Partial Reprogramming）是其在抗衰老领域的核心应用：不需要完全去分化，只需短暂激活OSKM以重置表观遗传时钟。

## 核心原理

OSKM因子（Oct4, Sox2, Klf4, c-Myc）将体细胞重编程为胚胎样状态时可：
- **恢复端粒长度**（体细胞端粒缩短是衰老标志，iPSC重建端粒酶活性）
- **改善线粒体功能**（ATP产生增加，氧化应激减少）
- **重置表观遗传时钟**（DNA甲基化模式恢复年轻化状态）
- **减少SASP**（衰老相关分泌表型，即炎症老化的驱动因子）

## 完全重编程 vs 部分重编程

| 类型 | 机制 | 风险 | 抗衰老应用 |
|------|------|------|-----------|
| 完全重编程（iPSC） | 体细胞→多能干细胞 | 成瘤性风险高 | 细胞疗法、疾病建模 |
| **部分重编程** | 短暂OSKM激活（如2天开/5天关，周期性） | 风险显著降低 | **直接年龄逆转策略** |

**Ocampo等2016年发表于Cell**：循环性OSKM激活使衰老小鼠（早老症模型）寿命延长30%，恢复肌肉再生能力，降低衰老标志物。

## 部分重编程的表观遗传效应

- 重置DNA甲基化时钟（Horvath时钟年龄降低）
- **同时保持细胞身份**（成纤维细胞仍为成纤维细胞，但表观遗传年龄重置）
- 减少SASP分泌（IL-6、IL-8等促炎因子下降）
- 人类细胞完整重编程约需30天（小鼠约20天）

## 主要挑战

**成瘤性风险**：
- c-Myc是原癌基因，过度激活可导致肿瘤表型
- 解决方案：非整合递送系统（mRNA、小分子）、自杀基因（异常增殖时触发清除）

**表观遗传记忆**：
- 体细胞来源的甲基化印记在重编程后残留
- 来源细胞类型影响分化倾向（血液来源→更强造血潜能）
- 解决方案：CRISPR表观遗传编辑（dCas9-DNMT3A/CRISPRoff精确靶向残留印记）

**功能未成熟性**：
- iPSC衍生细胞常表现为胎儿样表型
- 心肌细胞缺乏T小管；神经元突触连通性受限

**免疫兼容性**：
- 即使自体iPSC也可能因表观遗传异常触发T细胞反应

## CRISPR表观遗传编辑

与传统基因编辑不同，CRISPR表观遗传编辑**不切割DNA**，而是精确修改甲基化/乙酰化状态：

- **dCas9-DNMT3A**：靶向添加DNA甲基化（沉默特定年龄相关基因）
- **CRISPRoff**：更稳定的高甲基化，适合长效表观遗传时钟重置
- **dCas9-p300**：添加组蛋白乙酰化→激活目标基因转录

挑战：脱靶效应（高保真度HiFi-Cas9变体缓解）；编辑状态可能随时间回复（可能需要联合小分子如NAD+促进剂以维持稳定性）。

## 临床转化路线

1. 体外iPSC疗法：患者细胞重编程→分化为目标细胞类型→回输（自体疗法）
2. 部分重编程体内递送：mRNA/脂质纳米颗粒递送短暂OSKM表达
3. 纯小分子诱导：化学鸡尾酒替代蛋白质因子（更安全，无病毒载体）

"器官芯片"（Organ-on-a-chip）平台正在开发用于复制人类衰老生理，弥补小鼠模型局限性（小鼠体细胞保留端粒酶活性，不能完全代表人类衰老）。

## 关联概念

- [[Biological Age Reversal]] — iPSC/部分重编程是逆转生物年龄最前沿的技术方向
- [[Epigenetic Aging Clock]] — 部分重编程通过DNA甲基化时钟验证年龄逆转
- [[Gene Therapy Follistatin]] — 同样属于基因/细胞技术干预衰老的领域
- [[NAD+ 与衰老]] — NAD+促进剂可能有助于维持CRISPR表观遗传编辑的稳定性
- [[Longevity Drug Landscape 2025]] — iPSC作为未来治疗平台，与当前药物干预互补

[Source: raw/Decoding Aging through iPSC Reprogramming Advances and Challenges.md]
