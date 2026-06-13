---
name: Seed_Tech_CValueParadoxDSBRepair_20260611
description: Genome size across eukaryotes is not determined by gene count or organismal complexity but by species-specific DNA double-strand break repair behavior — organisms with larger genomes make smaller deletions, causing net genome growth over evolutionary time
type: seed
concept: C-Value Paradox Mechanistic Resolution via DSB Repair Bias
hook_insight: Why does a salamander have 10x more DNA than a human, despite being less complex? The answer may be simple: salamander cells make smaller deletions when repairing broken DNA. Accumulated over millions of generations, smaller deletions plus occasional insertions cause net genome expansion — genome size is a readout of repair precision, not biological sophistication
wiki_link: "[[DSB Repair Species Specificity]]"
---

# C-Value Paradox Mechanistic Resolution via DSB Repair Bias

## Technical Core Logic

The C-value paradox: eukaryotic genome sizes span 7,000-fold (from 2.5 Mb to 150 Gb) with no correlation to morphological or biochemical complexity. A lungfish has 40x more DNA than a human.

The DSB repair solution: species-specific differences in double-strand break (DSB) repair systematically bias genomes toward growth or contraction over evolutionary time.

## Key Experimental Evidence (Arabidopsis vs. Tobacco)

| Parameter | Tobacco (~5,000 Mb genome) | Arabidopsis (~135 Mb genome) |
|-----------|---------------------------|-------------------------------|
| Average deletion per DSB | ~920 bp | ~1,341 bp |
| Filler sequence insertions | 40% of events | 0% detected |
| Result | Net genome gain per event | Net genome loss per event |

- Arabidopsis (small genome) makes larger deletions with no insertions → net sequence loss
- Tobacco (large genome) makes smaller deletions plus inserts nuclear DNA fragments → net sequence gain
- The correlation is INVERSE: bigger genome → smaller deletions

## The Positive Feedback Loop

Large genome → tolerates more noncoding DNA → DSB repair is less aggressive with deletions → genomes grows further over time. Small genome → repair under stronger selective pressure to minimize insertions → genome stays compact.

## Two Mechanisms Acting in Parallel

| Mechanism | Direction | Examples |
|-----------|-----------|---------|
| Retrotransposon insertion | Genome expansion | 45% of human genome; >80% of maize |
| DSB repair deletion bias | Contraction or expansion | Species-specific, this seed's focus |

DSB repair bias provides a second, mechanistically distinct explanation for C-value variation, operating independently of transposon activity.

[Source: raw/Species-specific double-strand break repair and genome evolution in plants.md]
