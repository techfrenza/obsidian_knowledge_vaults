---
title: "Genomic 3D Structure and CRISPR Efficiency"
parent: "[[CRISPR-Cas9]]"
tags: [crispr, 3d-genome, hi-c, editing-efficiency, chromatin]
source: "raw/Strong association between genomic 3D structure and CRISPR cleavage efficiency.md"
---

# Genomic 3D Structure and CRISPR Efficiency

The 3D spatial organization of the genome—measured via Hi-C interaction data—is significantly correlated with CRISPR-Cas9 cleavage efficiency: target sites in spatially dense chromatin regions show lower editing efficiency.

## Key Finding

- 3D density features ranked in top 5–13% of 434 features tested against in-vivo CRISPR datasets
- Sites in low-density (sparse) genomic regions show consistently higher Cas9 cleavage efficiency
- Adding a single 3D feature improved LASSO model r² by 6% and xgboost r² by 11.8% in the Leenay dataset

## How 3D Features Are Calculated

| Feature Type | Definition | Relation to Density |
|-------------|-----------|-------------------|
| Distance (n) | Average distance to n closest genomic bins (Hi-C graph paths) | Inversely proportional to density |
| Radius (m) | Count of bins within fixed radius | Proportional to density |

- Based on 10 kb resolution Hi-C data; coarser resolutions (25kb, 50kb) reduce predictive value
- Distance estimated via 5 shortest graph paths (≤2 edges) to smooth Hi-C noise

## Proposed Mechanisms

1. **PAM competition**: Dense regions contain abundant NGG sequences; Cas9 samples more non-target sites before finding the correct PAM, reducing effective cleavage probability
2. **Physical accessibility**: Compact chromatin reduces Cas9 complex access even when the site is epigenetically open

## Orthogonality to Existing Features

- 3D density correlation with CRISPR efficiency is independent of gene expression levels (partial correlation unchanged when controlling for expression)
- Adds information not encoded by epigenetic features (DNase, H3K4me3, CTCF)
- Partial correlation against 5 state-of-the-art CRISPR models remains ~0.12 (unchanged)

## Practical Implications

- Prefer target sites in genomic "open space" (low Hi-C density) when designing sgRNAs
- 3D features from non-matching cell types partially transfer (useful when cell-specific Hi-C unavailable)
- TAD and A/B compartment features (coarser scale) are less predictive than 10 kb Hi-C features

## Connection to Physical Access Models

Both [[G-Quadruplex–CRISPR Interference]] and [[Noncanonical DNA Structures]] provide complementary physical-access barriers to Cas9: G4s obstruct at the local sequence level (single-bp scale), while 3D density obstructs at the chromosomal domain level (10 kb scale). These are independent, additive barriers. Regions near [[Telomere|telomeres]] combine both: high G4 density and high 3D compaction — explaining the extreme difficulty of telomeric CRISPR editing.

[Source: raw/Strong association between genomic 3D structure and CRISPR cleavage efficiency.md]
