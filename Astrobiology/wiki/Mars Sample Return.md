---
title: "Mars Sample Return"
parent: "[[Perseverance Rover]]"
tags: [Mars, sample-return, mission, NASA, ESA]
source: "raw/NASA Astrobiology.md, raw/Digging In When Rovers Get Dirt on Mars - S4E11.md, raw/Perseverance finds potential biosignatures in Jezero Crater.md, raw/Where we might find aliens in the next decade.md"
---

# Mars Sample Return

Mars Sample Return (MSR) is a multi-mission campaign to retrieve rock cores collected by [[Perseverance Rover]] from [[Jezero Crater]] and return them to Earth laboratories for definitive analysis.

## Mission Architecture (Three Legs)

| Leg | Mission | Role |
|-----|---------|------|
| 1 | Perseverance (active) | Collect samples, depot cache at Three Forks |
| 2 | Sample Retrieval Lander | Transfer tubes from rover; launch to Mars orbit via sample rocket |
| 3 | Earth Return Orbiter | Intercept container in Mars orbit; return to Earth (~2030s) |

A backup sample depot at "Three Forks" duplicates the on-rover cache in case retrieval from Perseverance fails.

## Why Earth Return Is Essential

- [[Perseverance Rover]] instruments are less sensitive than Earth labs
- Definitive tests for [[Cheyava Falls and Bright Angel Formation]] biosignature require isotopic analysis (Fe, S, C)
- Sample dating, organic biomarker analysis, and micro-scale imaging require instruments too heavy/power-intensive for Mars
- Mars rocks preserved unchanged for 3–4 billion years → irreplaceable record

## Status and Challenges (2024–2026)

- NASA-ESA formal agreement: October 2022
- Mission faces funding pressure under Trump administration NASA budget cuts
- Sample transfer system engineering ongoing (Aaron Yazzie, JPL team)
- Target return: early 2030s (timeline at risk)

[Source: raw/Where we might find aliens in the next decade.md]

## Scientific Value

Samples contain:
- [[Cheyava Falls and Bright Angel Formation|Cheyava Falls]] (Sapphire Canyon) — potential biosignature
- Igneous crater floor — Jezero formation history
- Delta sediments — habitability record
- Margin unit — water-rock alteration chemistry
- Witness tubes — contamination baseline

## Earth Entry System: Planetary Protection Reliability

The Earth Entry System (EES) must protect Earth from uncontained Martian material. Quantitative planetary protection target: **P(containment failure) ≤ 10⁻⁶** (one in a million).

A Bayesian uncertainty quantification (UQ) study (Sanderson et al., 2024, arXiv:2408.10083) modeled the EES reliability using:
- **Gaussian Process surrogate** for LS-DYNA impact simulations (25 simulation runs)
- **Bayesian posterior distributions** for 15 material property inputs (Young's Modulus, shear modulus, tensile/compressive strengths of IM7 carbon fiber and Kevlar)
- **Kriging prediction** for peak orbital sample container acceleration upon Earth landing
- Critical threshold: 3,000 G peak acceleration

Key result: the Bayesian approach (Setting B) achieves a median P(containment not assured) below the 10⁻⁶ target, though uncertainty bounds remain wide due to the small simulation dataset (25 runs). The frequentist approach (Setting A) fails to meet the target at median. More simulation runs are recommended.

This analysis informs the design feasibility of meeting backward planetary protection requirements.

## Linked Concepts

- [[Perseverance Rover]] — sample collector
- [[Jezero Crater]] — collection site
- [[Cheyava Falls and Bright Angel Formation]] — highest-priority sample
- [[Biosignature]] — what samples may confirm or rule out
- [[CoLD Scale]] — where confirmation would move the scale
- [[Mars Exploration History]] — mission in historical context
