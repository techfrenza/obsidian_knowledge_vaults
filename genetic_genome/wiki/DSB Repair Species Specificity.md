---
title: "DSB Repair Species Specificity"
parent: "[[Nonhomologous End Joining (NHEJ)]]"
tags: [dna-repair, genome-evolution, dsb, nhej, c-value-paradox]
source: "raw/Species-specific double-strand break repair and genome evolution in plants.md"
---

# DSB Repair Species Specificity

Different species exhibit fundamentally distinct double-strand break (DSB) repair outcomes, with direct consequences for genome size evolution. This was demonstrated in Arabidopsis thaliana vs. tobacco, two dicot plants differing >20-fold in genome size.

## Key Differences Between Arabidopsis and Tobacco

| Parameter | Tobacco (large genome, ~5,000 Mb) | Arabidopsis (small genome, ~135 Mb) |
|-----------|----------------------------------|-------------------------------------|
| Average deletion size | ~920 bp | ~1,341 bp |
| Filler sequence insertions | 40% of events | 0% detected |
| Micro-homology use in junctions | Similar (~60%) | Similar (~60%) |

- Arabidopsis consistently produces larger deletions and no filler insertions
- Tobacco inserts nuclear DNA sequences into break sites (filler sequences), increasing genome complexity

## Evolutionary Implication: Inverse Genome Size–Deletion Length Correlation

- Inverse correlation between genome size and average deletion length mirrors theoretical predictions from insect genomes (Petrov et al., 2000)
- Larger genomes tolerate smaller deletions + insertions → net genome expansion over time
- Smaller genomes show larger deletions + no insertions → net genome contraction or maintenance
- Species-specific DSB repair pathways may be the molecular mechanism driving C value paradox

## C Value Paradox

The C value paradox refers to the lack of correlation between genome size and organismal complexity across eukaryotes. [[DSB Repair Species Specificity]] repair bias (deletion size + filler insertion rate) provides a mechanistic explanation: different repair fidelity regimes create systematic genome size drift over evolutionary timescales.

- Retrotransposon insertion → genome enlargement (a second, well-known mechanism)
- DSB repair deletion bias → genome contraction (newly clarified mechanism)
- See also [[Mutation Rate vs Fidelity Trade-off]] for broader error-tolerance principles

## Molecular Cause (Hypothesis)

Species-specific differences may trace to:
- Different exo/endonuclease activity at break ends
- Different protection of broken ends against degradation
- DNA synthesis-dependent strand annealing mechanism active in tobacco but not Arabidopsis
- Related pathway: [[Homology-Mediated End Joining (HMEJ)]] in plants uses DSB-mediated integration with short homology arms

[Source: raw/Species-specific double-strand break repair and genome evolution in plants.md]
