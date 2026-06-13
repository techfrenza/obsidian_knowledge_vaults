---
title: "D-Statistic (ABBA-BABA Test)"
parent: "[[Introgression]]"
tags: [evolutionary-genetics, introgression, statistics, phylogenetics]
source: "raw/Ghost Lineages Highly Influence the Interpretation of Introgression Tests.md"
category: "Evolutionary Genetics"
date: "2026-06-10"
---

# D-Statistic (ABBA-BABA Test)

The D-statistic (Patterson's D, ABBA-BABA test) is the most widely used statistical method for detecting [[Introgression]] between lineages. It compares frequencies of two biallelic SNP patterns in a 4-taxon (quartet) phylogenetic framework.

## How It Works

Given a quartet ((P1, P2), P3, Outgroup):
- **ABBA**: P1 shares derived allele B with P3 (and P2 has ancestral A)
- **BABA**: P2 shares derived allele B with P3 (and P1 has ancestral A)
- Under no gene flow, ABBA = BABA (due to incomplete lineage sorting only)
- Significant D ≠ 0 indicates excess of one pattern → gene flow between P3 and P1 (or P2)

Formula: D = (ABBA - BABA) / (ABBA + BABA)

## Key Strengths

- Computationally fast
- Distinguishes introgression from incomplete lineage sorting (ILS)
- Widely implemented and validated

## Critical Weakness: Ghost Lineages

[[Ghost Lineage|Ghost lineage]] introgression from a "midgroup" lineage (between ingroup and outgroup) produces the same ABBA/BABA imbalance as ingroup introgression. This means:

- **Both the donor AND the recipient are misidentified**
- Error rate increases with: (1) proportion of unsampled lineages, (2) outgroup distance, (3) probability of cross-lineage introgression
- Using a **distant outgroup** (standard recommendation) actually **increases** error rate for ghost introgression
- With 25% species sampling (realistic for most groups), >50% of significant D-statistics may reflect ghost introgression

## Real-World Impact

- Neanderthal-human introgression study (Green et al. 2010): outgroup distance ratio = 0.873 (high error-risk zone)
- Bear introgression studies: removing one lineage from the dataset reverses the D-statistic conclusion

## Recommended Response

Report significant D-statistics with ghost introgression as an **equally probable alternative** scenario, not merely as a caveat. Develop combinatorial algorithms that jointly analyze all quartets to minimize ghost-introgression misinterpretation.

[Source: raw/Ghost Lineages Highly Influence the Interpretation of Introgression Tests.md]
