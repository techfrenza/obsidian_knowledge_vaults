---
title: "Nonhomologous End Joining (NHEJ)"
parent: "[[CRISPR-Cas9]]"
tags: [dna-repair, nhej, gene-editing, double-strand-break]
source: "raw/Gene editing and CRISPR-dependent homology-mediated end joining.md"
category: "Gene Editing"
date: "2026-06-10"
---

# Nonhomologous End Joining (NHEJ)

NHEJ is the predominant double-strand break (DSB) repair pathway in higher eukaryotes. It joins broken DNA ends without requiring sequence homology, but is error-prone, frequently introducing small insertions or deletions (indels).

## Two Sub-pathways

| Pathway | Factors | Features |
|---------|---------|---------|
| Classical NHEJ (C-NHEJ) | Ku70/Ku86 heterodimer, DNA-PKcs, LIG4 | Active in G1/early S phase; can be precise or generate indels |
| Alternative NHEJ (A-NHEJ/MMEJ) | POLQ (Pol theta), LIG1/LIG3 | Often uses short microhomologies (≥3 nt); more error-prone; active in mitosis |

## Role in [[CRISPR-Cas9]] Knockouts

CRISPR-Cas9 introduces a DSB; NHEJ-mediated repair generates indels at the cut site. Indels in coding exons cause frameshift mutations → premature stop codons → gene knockout. NHEJ is efficient enough to achieve biallelic modification in one step.

## Why NHEJ Dominates in Mammals

Mammalian cells evolved to have abundant noncoding DNA, which may have facilitated tolerance of imprecise end joining. NHEJ must be suppressed to enable [[Homology-Directed Repair (HDR)]] for precise knock-in.

## NHEJ vs Bacteria/Lower Eukaryotes

Bacteria and yeast preferentially use [[Homology-Directed Repair (HDR)]] for DSB repair. The evolution of NHEJ dominance in higher eukaryotes reflects their larger genomes, more repetitive DNA, and different cell cycle requirements.

## [[DNA Palindrome]] Connection

Palindrome instability in bacteria involves a NHEJ-independent mechanism. In humans, center-break palindrome modification (see [[NF1 Locus Palindrome]]) bears hallmarks of NHEJ, indicating NHEJ also drives palindrome evolution in human cells.

[Source: raw/Gene editing and CRISPR-dependent homology-mediated end joining.md]
