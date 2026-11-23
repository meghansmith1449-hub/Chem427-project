# 008: Part A1–A2 of project11.ipynb, AFM − SEM for silica and gold

- **Status:** done
- **Created:** 2026-11-21
- **Updated:** 2026-11-22
- **Due:** 2026-11-22

## Goal

Section A1–A2 of `project11.ipynb` gives, for silica and each gold population, the median AFM height, the median SEM minimum Feret and their difference in nm, with a bootstrap 95% CI by image (the result) and by particle (the named alternative), shown in one figure and followed by a sentence giving the numbers.

## Progress log

### 2026-11-21

- Built A1–A2: median AFM height − median SEM minimum Feret (N1), bootstrap 95% CI drawing whole images, SEM within each laboratory (N2, N5), with the particle bootstrap beside it (N5). 10,000 resamples, seed 427.
- Silica: AFM 43.72 nm (42 particles, 10 images), SEM 47.74 nm (287, 13); AFM − SEM = −4.01 nm, CI −4.78 to −2.68 nm by image, −5.03 to −2.47 nm by particle.
- Gold 30 nm: AFM 28.70 nm (324, 20), SEM 30.71 nm (122, 10, LNE only); −2.01 nm, CI −2.35 to −0.99 by image, −2.30 to −0.53 by particle.
- Gold 60 nm: AFM 58.66 nm (296, 20), SEM 61.42 nm (196, 16); −2.76 nm, CI −4.10 to −1.54 by image, −4.02 to −2.09 by particle.
- Every CI excludes 0 under both bootstrap units, so the choice of unit does not change the conclusion.

### 2026-11-22

- Wrote the Figure 2 caption and added the reason for the method to the A1–A2 cell: long tails (gold 60 nm AFM 45.7 to 105.2 nm), only 42 silica AFM particles in 10 images, and BAM and LNE medians that differ for gold 60 nm (64.3 vs 60.0 nm).

## Outcome

Done. AFM reads smaller than SEM for all three populations: −4.01 nm for silica, −2.01 nm for gold 30 nm and −2.76 nm for gold 60 nm, and every 95% CI excludes 0 by image and by particle. Whether these differences exceed the combined uncertainty (A4), and the comparison with SAXS (A3), are left for the final.
