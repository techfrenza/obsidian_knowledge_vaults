---
title: "Homology-Mediated End Joining (HMEJ)"
parent: "[[CRISPR-Cas9]]"
tags: [dna-repair, gene-editing, gene-therapy, knock-in]
source: "raw/Gene editing and CRISPR-dependent homology-mediated end joining.md"
category: "Gene Editing"
date: "2026-06-10"
---

# Homology-Mediated End Joining (HMEJ)

HMEJ is a [[CRISPR-Cas9]]-enabled knock-in mechanism that achieves high-frequency (20–100%), precise genome editing using linear dsDNA donors with short homology arms (~24–48 bp). It mechanistically resembles single-strand annealing (SSA) rather than canonical [[Homology-Directed Repair (HDR)]].

## Mechanism

1. CRISPR makes 3 DSBs: 2 cuts liberate a linear donor from a plasmid; 1 cut at the chromosomal target site
2. All 4 ends undergo 5'→3' resection, exposing ssDNA regions of homology
3. RAD52-dependent (not RAD51-dependent) annealing of complementary homology regions
4. Flap trimming → gap filling → ligation → seamless integration

## Key Advantages Over Canonical HDR

| Feature | HDR | HMEJ |
|---------|-----|------|
| Homology arm length | Hundreds to thousands of bp | 24–48 bp |
| Frequency | 1–5% | 20–100% |
| Large cargo | Difficult (donor DNA delivery) | Feasible (6+ kb demonstrated) |
| RAD51 requirement | Yes | No |
| Precision (indels at junction) | Low background | ~0.5–1.5% indels |

## Proposed Molecular Basis

HMEJ is mechanistically analogous to SSA (single-strand annealing): SSA normally joins homologous sequences flanking a single DSB; HMEJ extends this to four ends from two DSBs. This explains the short homology requirement — as short as 29 bp is functional in SSA. RAD51-independence has been confirmed.

## Therapeutic Significance

HMEJ solves two critical barriers in gene therapy:
1. **Large cargoes**: cDNAs >6 kb can be integrated (covering most human genes)
2. **Complementation approach**: Instead of correcting each patient's unique mutation, insert a full-length wild-type cDNA at the native locus under the endogenous promoter. Applicable to Fanconi anemia, muscular dystrophy, BRCA1/2-associated cancers.

[Source: raw/Gene editing and CRISPR-dependent homology-mediated end joining.md]
