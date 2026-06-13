---
title: "Transmission Spectroscopy"
parent: "[[JWST]]"
tags: [method, spectroscopy, exoplanet, atmosphere]
source: "raw/New Constraints on DMS and DMDS in the Atmosphere of K2-18 b from JWST MIRI.md, raw/Strongest Hints Yet Of Biological Activity Outside Our Solar System.md"
---

# Transmission Spectroscopy

Transmission spectroscopy is the primary technique for characterizing exoplanet atmospheres. During planetary transit (when the planet passes in front of its host star), starlight filtering through the planetary atmosphere imprints molecular absorption features into the stellar spectrum.

## How It Works

1. Star emits broadband light
2. Planet transits → blocks a fraction of starlight
3. A small portion of starlight passes through the planet's atmospheric limb (terminator region)
4. Molecules absorb specific wavelengths → "imprints" in the received spectrum
5. Difference between in-transit and out-of-transit spectra reveals atmospheric composition

## Signal Size

Transit depth variation: typically 200–600 ppm for sub-Neptunes like [[K2-18b]].
This requires extremely stable photometry and multiple transit observations to achieve sufficient signal-to-noise.

## What Can Be Detected

| Species | Wavelength | Notes |
|---------|-----------|-------|
| CH₄ | 3.3 μm | Strong near-IR feature |
| CO₂ | 4.3 μm | Detected in K2-18b at 3σ |
| H₂O | 1.4 μm | Condensed below detection altitude in K2-18b |
| DMS | 3.3, 6.8–8, 9–10 μm | Overlaps CH₄ in near-IR; cleaner in MIRI |
| DMDS | 6.8–8, 10–11 μm | Best resolved in mid-IR |

## Advantages of Hycean Worlds

[[Hycean World]] planets have:
- H₂-rich atmosphere → lower molecular weight → larger atmospheric scale height
- Larger planet size → greater transit depth
- Both effects amplify the transmission signal by factors of 5–10× vs. Earth-like planets

## Data Reduction Challenges

- Instrument systematics (detector settling, correlated noise)
- Spectral degeneracy between overlapping molecular features (DMS vs. DMDS vs. CH₄)
- Flux offsets between detector segments
- Limb darkening corrections

For [[K2-18b]] MIRI analysis: two independent pipelines (JExoRES, JexoPipe) used; results stable across binning, trend models, noise treatments.

## Disequilibrium Chemistry as Life Indicator

Key signature is **atmospheric disequilibrium**: gases that cannot coexist at observed ratios without continuous replenishment.
- CH₄ + CO₂ + absence of CO: more easily explained by inhabited hycean than uninhabited case
- DMS/DMDS at >10 ppm: requires active biogenic source flux ≳20× Earth

## Linked Concepts

- [[JWST]] — primary instrument
- [[K2-18b]] — leading application
- [[DMS and DMDS]] — key detected molecules
- [[Hycean World]] — optimal planet class for technique
- [[Biosignature]] — what the technique seeks
- [[Extremely Large Telescope]] — future complement using direct imaging instead
