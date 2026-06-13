---
title: "Ghost Lineage Effect on Branch Lengths"
parent: "[[Ghost Lineage]]"
tags: [phylogenetics, ghost-lineage, gene-flow, branch-lengths, introgression]
source: "raw/Ghost lineages can invalidate or even reverse findings regarding gene flow.md"
---

# Ghost Lineage Effect on Branch Lengths

Ghost lineages (extinct, unsampled, or unknown taxa) distort the expected relationship between branch lengths in gene trees and gene flow events, potentially reversing published conclusions about the timing and directionality of horizontal gene flow (HGF).

## Core Problem

In standard phylogenetics, shorter branch lengths in gene trees relative to species trees signal HGF — because transferred genes reflect the divergence time of donor and recipient, not their speciation time. This expectation **breaks down entirely** when the donor is a ghost lineage.

| Scenario | Branch Length Signal | Correct Interpretation |
|---------|---------------------|----------------------|
| HGF between sampled taxa | Short branch in gene tree | Standard interpretation valid |
| HGF from ghost donor → sampled recipient | May appear as LONG branch | Misinterpreted as absence of HGF |
| Two events (early A, late B) with ghosts | Branch order may be REVERSED | Wrong temporal ordering of events |

## Empirical Reanalyses Affected

The D3 method (branch-length-based introgression test) systematically fails under ghost lineage conditions:
- Malaria vector (*Anopheles*) branching order conclusions become unfounded
- Eukaryogenesis gene acquisition timing (FECA to LECA) may be reversed
- Chloroplast membrane gene acquisition ordering may be wrong

## Simulation Evidence

Under realistic ghost taxon loads (>75% unsampled), the expected correlation between HGF timing and branch lengths is completely lost. Even the D-statistic (ABBA-BABA) companion metric shows >50% misinterpretation rate under these conditions (see [[D-Statistic (ABBA-BABA Test)]]).

## Proposed New Interpretation Rule

Any signal of HGF detected via branch lengths should be interpreted **first** as evidence of ghost lineage involvement rather than direct gene flow between sampled taxa. The ghost hypothesis is the null; ingroup introgression is the exception to demonstrate.

## Relationship to Introgression Detection Methods

This branch-length failure mode is complementary to the well-characterized [[D-Statistic (ABBA-BABA Test)]] ghost-lineage problem. Both methods are confounded by [[Introgression]] involving unsampled ghost donors. Together, they indicate that all gene-flow detection methods require systematic ghost-lineage accounting. The existence of ghost donors is the expected condition in any [[Phylogenetics]] study — >99.9% of species that have ever lived are extinct.

[Source: raw/Ghost lineages can invalidate or even reverse findings regarding gene flow.md]
[Source: raw/Ghost Lineages Highly Influence the Interpretation of Introgression Tests 1.md]
