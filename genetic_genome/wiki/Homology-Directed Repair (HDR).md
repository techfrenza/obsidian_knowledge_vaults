---
title: "Homology-Directed Repair (HDR)"
parent: "[[CRISPR-Cas9]]"
tags: [dna-repair, hdr, gene-editing, gene-therapy]
source: "raw/Gene editing and CRISPR-dependent homology-mediated end joining.md"
category: "Gene Editing"
date: "2026-06-10"
---

# Homology-Directed Repair (HDR)

HDR is a precise DSB repair pathway that uses a homologous template (sister chromatid or donor DNA) to restore the original or modified sequence. It is the basis for precise knock-in gene editing but occurs at low frequency in higher eukaryotes.

## Canonical HDR Mechanism

1. DSB ends undergo 5'→3' resection → 3' single-stranded overhangs
2. RPA coats ssDNA overhangs
3. RAD51 (key strand-exchange protein, with BRCA1/2 as accessories) loads onto ssDNA and performs homology search
4. Strand invasion into donor template
5. DNA synthesis from 3' end; resolution replaces chromosomal sequence with donor sequence

## Why HDR Is Rare in Somatic Cells

HDR is restricted mainly to S/G2 phase (when sister chromatid is available). [[Nonhomologous End Joining (NHEJ)]] competes and dominates, especially in G1. Overall knock-in frequency is typically 1–5%.

## Donor DNA Types

| Type | Size | Features |
|------|------|---------|
| Large dsDNA | kb-range | High innate immune activation; labor-intensive |
| ssODN (oligodeoxynucleotides) | ~100 nt | Avoids immune activation; limited payload (~30 nt new sequence) |

## Enhancement Strategies

- NHEJ inhibitors (e.g., M3814, Scr7)
- HDR activators (RS-1, RAD51 enhancers)
- ALT-R HDR Enhancer V2 (IDT): C-NHEJ inhibitor; increases HDR up to 44-fold in some cell lines
- Cell-cycle synchronization to G2/S phase

## Therapeutic Limitation

Many genetic diseases involve large genes with diverse mutations. Correcting each patient's unique mutation requires individualized reagents. [[Homology-Mediated End Joining (HMEJ)]] addresses this by enabling large cDNA complementation with high efficiency.

[Source: raw/Gene editing and CRISPR-dependent homology-mediated end joining.md]
