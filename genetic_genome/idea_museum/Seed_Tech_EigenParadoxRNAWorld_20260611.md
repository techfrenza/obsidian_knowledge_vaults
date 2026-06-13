---
name: Seed_Tech_EigenParadoxRNAWorld_20260611
description: The RNA World's foundational paradox — accurate replication requires molecular machinery, but that machinery requires accurate replication to evolve — defines an information-collapse upper bound that constrained the origin of life
type: seed
concept: Eigen's Error Threshold as RNA World Constraint
hook_insight: Life needed accurate replication before it could evolve the proteins that enable accurate replication — Eigen's paradox sets the maximum mutation rate the first self-replicating molecules could tolerate before genetic information collapsed entirely, creating a hard upper bound on the RNA World's viability window
wiki_link: "[[Mutation Rate vs Fidelity Trade-off]]"
---

# Eigen's Error Threshold as RNA World Constraint

## Technical Core Logic

The mutation rate vs. fidelity trade-off is not just an evolutionary optimization — at its extreme, it defines whether life is possible at all. Eigen's paradox (error threshold) states:

- A genome can only be faithfully replicated if its length × per-base mutation rate < 1 error per replication
- Exceed this threshold and information content collapses: every offspring is a mutant, the sequence converges to random noise, the replicator dies
- Early RNA molecules had no proofreading polymerases — those are protein-based enzymes that themselves require accurate replication to evolve
- Therefore: the first RNA replicators had to be short enough to stay below the error threshold with zero enzymatic support

## The Paradox Structure

| Requirement | Problem |
|-------------|---------|
| Accurate replication | Requires proofreading enzymes |
| Proofreading enzymes | Require large, accurately replicated genomes |
| Large genomes | Exceed error threshold without proofreading |
| Short RNA tolerable | Too small to encode proofreading machinery |

## Resolution Pathway

- RNA World hypothesis: early replicators were ribozymes — RNA molecules with enzymatic activity
- Ribozyme-based replication could provide minimal proofreading, expanding the tolerable genome size
- Quasispecies theory: the replicating unit is not a sequence but a cloud of mutants centered on the "master sequence"
- Selection acts on the quasispecies as a whole — some mutation is tolerated and even beneficial for exploration

## Comparison to Modern Systems

| System | Error Rate | Proofreading |
|--------|-----------|-------------|
| RNA viruses (modern) | ~10⁻³–10⁻⁵ /bp | None or minimal |
| Early RNA World (model) | >10⁻³ | None |
| Modern DNA polymerase | ~10⁻⁷–10⁻⁸ /bp | 3'→5' exonuclease |
| With MMR (mismatch repair) | ~10⁻⁹–10⁻¹⁰ /bp | Post-replication correction |

The transition from RNA to DNA genomes was partly driven by the 10x lower mutation rate of deoxyribonucleotides — DNA's sugar chemistry makes it more stable, allowing larger, more complex genomes below the error threshold.

[Source: raw/基因组工程中的错误权衡、非规范DNA结构与CRISPR编辑精准性：2026年最新研究洞见.md]
