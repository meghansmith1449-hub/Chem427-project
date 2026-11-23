# 006: Load section of project11.ipynb

- **Status:** done
- **Created:** 2026-11-21
- **Updated:** 2026-11-22
- **Due:** 2026-11-22

## Goal

The "Load the expert annotations" section of `project11.ipynb` reads `data/annotations.zip` by a relative path into a table with one row per expert-annotated particle in one image, sizes in nm. A printout gives the particle and image counts per material × technique for AFM and SEM. A numbered markdown list names each problem found on loading and what was done about it, citing the printed numbers.

## Progress log

### 2026-11-21

Restarted `project11.ipynb` (old version in `archive/project11-2026-11-21.ipynb`) with an outline in question order. Added a Setup and imports cell, `ANNOTATIONS_ZIP` and `TECHNIQUES` in Parameters, and the Load section: a tidy table of 11,227 particles × 10 columns, one row per expert-annotated particle, every unit checked as nm.

Loading checks on the 1,267 particles in scope:
- gold AFM 620 particles in 20 images, gold SEM 318 in 16, silica AFM 42 in 10, silica SEM 287 in 13;
- AFM height missing on 605 of 605 SEM rows and 0 AFM rows (structural);
- SEM pixel sizes 1.40 and 1.84 nm/px (gold), 0.92 and 1.86 nm/px (silica); AFM 5.87 nm/px;
- 0 impossible sizes, 0 particles counted twice;
- 558 non-particle JSON entries skipped.

Each problem and what was done is listed in the notebook. Outliers are left for the Explore section, after the histograms.

### 2026-11-21 (later)

Added item 6 (outliers kept, N6, checked after the histograms) and item 7 (not used: 2,318 TEM and 826 TSEM particles, N4, and 6,816 AFM and SEM particles of other nPSize materials) to the Problems list.

## Outcome

Done. The Load section reads `data/annotations.zip` into a table of 11,227 particles × 10 columns, units checked as nm, and keeps the 1,267 in scope (gold and silica, AFM and SEM). The Problems list has seven numbered items, each with its printed count and what was done; nothing is removed.
