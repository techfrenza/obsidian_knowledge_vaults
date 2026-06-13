---
title: "G-Quadruplex–CRISPR Interference"
parent: "[[CRISPR-Cas9]]"
tags: [g-quadruplex, crispr, dna-structure, editing-efficiency]
source: "raw/Encounters Between Cas9dCas9 and G-quadruplexes Implications for Transcription Regulation and Cas9-Mediated DNA Cleavage.md"
---

# G-Quadruplex–CRISPR Interference

G-quadruplexes (G4s) are [[Noncanonical DNA Structures|noncanonical DNA structures]] that physically obstruct [[CRISPR-Cas9]] activity by competing with R-loop formation, reducing cleavage efficiency and altering transcriptional outcomes.

## Mechanism of Interference

| G4 Location | Cas9 Effect | Explanation |
|-------------|------------|-------------|
| G4 in nontarget strand (NTS) | GQ forms readily; complex destabilized | R-loop frees NTS from Watson-Crick base-pairing, facilitating G4 folding |
| G4 in target strand (TS, low stability) | No GQ forms; R-loop maintained | R-loop force dominates over weak GQ stability |
| G4 in target strand (TS, high stability) | GQ partially maintained; FRET shows distortion | Stable GQ competes with R-loop, reducing cleavage efficiency significantly |

- Stable G4s in TS vicinity suppress Cas9 cleavage efficiency, especially on the G-rich strand
- G4-stabilizing agents (e.g., PhenDC3, K⁺ ions) amplify this inhibition
- Zn²⁺ abolishes Cas9 cleavage entirely (independent of G4 effects)

## dCas9 as Transcriptional Modulator via G4 Targeting

dCas9 (nuclease-dead Cas9) can up- or down-regulate transcription by targeting different regions of a promoter G4 sequence without introducing permanent mutations:
- Targeting the 3' side of TH49 (human tyrosine hydroxylase promoter): 276% upregulation by inhibiting competition from 3' GQ, facilitating 5' GQ formation
- Targeting the 5' side: reduces expression
- Advantage over small molecules: site-specific and potentially reversible

## Implications for Genome Editing

- Design sgRNAs to avoid high-stability G4 regions in the target strand
- G4 prediction tools (G4-seq data) should be incorporated in sgRNA design pipelines
- High-fidelity Cas9 variants + modified sgRNAs can partially compensate for G4-induced efficiency loss
- G4-rich regions ([[Telomere|telomeres]], oncogene promoters like c-MYC) require special strategies: auxiliary helicases, modified Cas9

[Source: raw/Encounters Between Cas9dCas9 and G-quadruplexes Implications for Transcription Regulation and Cas9-Mediated DNA Cleavage.md]
[Source: raw/Targeting G-quadruplex Forming Sequences with Cas9.md]
[Source: raw/Targeting specific DNA G-quadruplexes with CRISPR-guided G-quadruplex-binding proteins and ligands.md]
