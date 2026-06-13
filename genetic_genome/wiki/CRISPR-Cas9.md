---
title: "CRISPR-Cas9"
parent: "[[Molecular Biology]]"
tags: [gene-editing, crispr, dna-repair, gene-therapy]
source: "raw/Gene editing and CRISPR-dependent homology-mediated end joining.md"
category: "Gene Editing"
date: "2026-06-10"
---

# CRISPR-Cas9

CRISPR-Cas9 is an RNA-programmable restriction enzyme derived from the bacterial adaptive immune system, repurposed for precise genome editing in mammalian cells. Its 2013 application to human cells revolutionized molecular biology (Nobel Prize 2020: Charpentier and Doudna).

## Mechanism

1. A single guide RNA (sgRNA, ~100 nt with ~20 nt targeting sequence) directs Cas9 to a specific genomic locus
2. Cas9 introduces a blunt double-strand break (DSB) at the target
3. The DSB is repaired by either [[Nonhomologous End Joining (NHEJ)]] or [[Homology-Directed Repair (HDR)]], depending on cell state and available donor DNA

## Applications

| Goal | Pathway | Outcome |
|------|---------|---------|
| Knockout (gene inactivation) | [[Nonhomologous End Joining (NHEJ)]] | Indels → frameshift → loss of function |
| Knock-in (precise edit / gene therapy) | [[Homology-Directed Repair (HDR)]] | Exact sequence correction/insertion |
| Transcriptional control | dCas9 (dead Cas9) + activator/repressor | CRISPRa / CRISPRi — no DNA cleavage |
| Epigenome editing | dCas9 + histone-modifying enzymes | [[Post-Translational Modifications]] on histones |

## Advantages Over Prior Technologies

- ZFNs: difficult to engineer, marginal specificity
- TALENs: laborious, one TALEN per target site
- CRISPR-Cas9: programmable by sgRNA sequence alone; enables biallelic knockout and multiplexing in one experiment

## Key Challenge: NHEJ vs HDR Dominance

Higher eukaryotes preferentially use NHEJ (error-prone) over HDR (precise), by at least 1,000-fold. Knock-in approaches require strategies to suppress NHEJ or enhance HDR, including small molecule inhibitors of NHEJ or HDR enhancers (e.g., ALT-R HDR Enhancer V2).

## Off-Target Activity

Cas9 can nick off-target genomic sites. Evolved high-fidelity Cas9 variants (eSpCas9, HiFi Cas9) reduce off-target activity to undetectable levels by whole-genome sequencing.

## G-Quadruplex and 3D Chromatin Effects on Efficiency

- Stable [[Noncanonical DNA Structures|G-quadruplexes]] in the target strand reduce Cas9 cleavage efficiency by competing with R-loop formation; see [[G-Quadruplex–CRISPR Interference]]
- The genomic 3D structure influences CRISPR cleavage: target sites in spatially dense chromatin regions show lower efficiency; see [[Genomic 3D Structure and CRISPR Efficiency]]
- dCas9 can up/down regulate transcription by targeting G4-containing promoters without DNA cuts

## HMEJ (Novel High-Efficiency Knock-in)

[[Homology-Mediated End Joining (HMEJ)]] uses CRISPR to cut both the chromosome and a donor plasmid, liberating a linear dsDNA donor with short (~40 bp) homology arms. This achieves >30% knock-in rates — far exceeding canonical HDR — and can incorporate large cargoes (>6 kb), making it suitable for therapeutic complementation.

[Source: raw/Gene editing and CRISPR-dependent homology-mediated end joining.md]
