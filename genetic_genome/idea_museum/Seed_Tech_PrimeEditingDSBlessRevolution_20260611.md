---
name: Seed_Tech_PrimeEditingDSBlessRevolution_20260611
description: Prime Editing achieves all 12 classes of point mutation plus small insertions and deletions without creating a double-strand break, rendering the fundamental NHEJ vs. HDR repair pathway trade-off that has constrained CRISPR gene therapy entirely irrelevant
type: seed
concept: Prime Editing Bypasses the NHEJ/HDR Trade-off
hook_insight: The central problem of CRISPR gene therapy is that Cas9 creates a double-strand break, and the cell's response (NHEJ or HDR) determines whether the edit is precise or error-prone — a coin flip the researcher cannot fully control. Prime Editing cuts only one strand and uses reverse transcriptase to write a new sequence directly, making the NHEJ/HDR pathway choice a non-issue for the first time in the field's history.
wiki_link: "[[Mutation Rate vs Fidelity Trade-off]]"
---

# Prime Editing Bypasses the NHEJ/HDR Trade-off

## Technical Core Logic

CRISPR-Cas9 gene therapy faces a fundamental repair pathway problem:

| Pathway | Frequency in Human Cells | Outcome |
|---------|--------------------------|---------|
| NHEJ | ~90% | Error-prone; insertions/deletions; unpredictable |
| HDR | ~1–5% | Precise, but requires donor template; too rare for therapy |

Every DSB-creating CRISPR strategy must navigate this trade-off. Attempts to bias toward HDR (using cell cycle synchronization, RAD51 activators, NHEJ inhibitors) have shown limited efficacy.

## Prime Editing Architecture

Prime Editing (PE) sidesteps the trade-off entirely:

1. Nickase Cas9 (nCas9) — cuts only the non-target strand (no DSB)
2. Reverse transcriptase (RT) domain — fused to nCas9
3. pegRNA (prime editing guide RNA) — encodes both the targeting sequence AND the desired edit as an RT template
4. The RT copies the pegRNA edit template directly into the nicked site
5. A second nick (PE3 strategy) biases mismatch repair to resolve the edit in the desired direction

## What Prime Editing Can Do Without a DSB

- All 12 possible single-base conversions (transversions and transitions)
- Small insertions (up to ~44 bp demonstrated)
- Small deletions (up to ~80 bp demonstrated)
- Combinations thereof

## Trade-off Comparison

| Approach | DSB? | HDR needed? | Repair pathway dependency |
|----------|------|-------------|--------------------------|
| SpCas9 + donor | Yes | Yes | High (HDR vs. NHEJ coin flip) |
| Base editing | No (nick) | No | Low (deaminase-based) |
| Prime editing | No (nick) | No | None for edit fidelity |

Connection: The [[Mutation Rate vs Fidelity Trade-off]] that governs genome evolution is recapitulated in engineered systems — Prime Editing is the first tool to achieve precision without the fidelity cost of inducing a DSB.

[Source: raw/基因组工程中的错误权衡、非规范DNA结构与CRISPR编辑精准性：2026年最新研究洞见.md]
