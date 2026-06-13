---
title: "Shelterin Complex"
parent: "[[Telomere]]"
tags: [telomere, shelterin, protein-complex, genome-stability, dna-damage-response]
source: "raw/Systematic analysis of human telomeric dysfunction using inducible telosomeshelterin CRISPRCas9 knockout cells.md"
---

# Shelterin Complex

The shelterin (telosome) complex is a six-subunit protein assembly that directly protects mammalian chromosome ends (telomeres) from DNA damage recognition and inappropriate repair, while also regulating telomerase access.

## Subunits and Binding Architecture

| Subunit | Direct Telomere Contact | Key Function |
|---------|------------------------|-------------|
| TRF1 | dsDNA via Myb domain | Length regulation |
| TRF2 | dsDNA via Myb domain | End protection; inhibits NHEJ |
| RAP1 | Via TRF2 (indirect) | Represses HDR at telomeres |
| TIN2 | TRF1 + TRF2 bridge | Structural hub; mitochondrial localization |
| TPP1 | Via TIN2 (indirect) | Telomerase recruitment |
| POT1 | ssDNA 3' overhang | G-overhang protection; length regulation |

- TIN2 bridges TRF1, TRF2, and the TPP1-POT1 heterodimer
- TPP1-POT1 heterodimer regulates telomerase access to the 3' overhang

## Human vs. Mouse Differences

- Human telomeres are considerably shorter than mouse laboratory telomeres
- Human has **one POT1 gene**; mouse has two (Pot1a, Pot1b) with distinct functions
- Homozygous KO of TRF2 and TIN2 is lethal in mice; human cells show different thresholds
- RNAi knockdown of TRF2 in human cells shows minimal DDR — residual protein is sufficient

## CRISPR-Based Systematic Analysis

Inducible CRISPR KO cell lines (doxycycline-inducible Cas9) enabled systematic loss-of-function analysis:
- Dual sgRNA strategy: simultaneous cleavage at two sites → high efficiency deletion, >80% alleles with frameshifting indels
- Essential genes (TRF2, TIN2) could be studied in inducible KO without lethality blocking experiments
- Revealed TIN2 localizes to mitochondria and regulates oxidative phosphorylation — metabolic control role

## Disease Relevance

Telomere dysfunction via shelterin dysregulation causes:
- Dyskeratosis congenita (DKC)
- Aplastic anemia
- Idiopathic pulmonary fibrosis
- Promotes cancer via genome instability (telomere fusions, crisis)

## Relationship to G-Quadruplex Biology

The 3' single-strand G-overhang protected by POT1 forms [[Noncanonical DNA Structures|G-quadruplex]] structures. TRF2 inhibits NHEJ ([[Nonhomologous End Joining (NHEJ)]]) at telomere ends — loss of TRF2 leads to end-to-end chromosome fusions via NHEJ. The shelterin complex is therefore the biological layer that prevents NHEJ from processing telomere DSB-mimics. CRISPR-based functional studies of shelterin subunits depend on telomere G-quadruplex biology interacting with Cas9 access (see [[G-Quadruplex–CRISPR Interference]]).

[Source: raw/Systematic analysis of human telomeric dysfunction using inducible telosomeshelterin CRISPRCas9 knockout cells.md]
