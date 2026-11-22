# 004: Set up project11.ipynb, load and explore the expert annotations

- **Status:** done
- **Created:** 2026-11-20
- **Updated:** 2026-11-21
- **Due:** 2026-11-22

## Goal

`project11.ipynb` has an availability statement, a source table with URLs and access dates, a Parameters section with a seeded generator, and a cell that loads the expert annotations for nPSize01 (gold) and nPSize10 (silica) from `data/annotations.zip`. The problems found on loading (missing values, varying pixel size, outliers, format quirks) are each reported with a count and what was done. A table of descriptive statistics by material × technique (AFM, SEM) and one histogram figure are each followed by a sentence giving the numbers. The first analysis (AFM vs SEM median with bootstrap CI) is task 005; own segmentation of the AFM maps is task 006.

## Progress log

### 2026-11-20

Created `project11.ipynb` in the top folder, with a title cell only.

Added the Data availability cell to `project11.ipynb`. Matched each zip in `data/` to its Zenodo record (VSL.zip → 5578927; LNE1–3.zip → 5578878; annotations.zip → 5577401); all five MD5 checksums match the values listed on Zenodo.

**Open question for mentor:** may the data zips (~225 MB total; largest file LNE2.zip at 69.6 MB, under GitHub's 100 MB limit) be committed in `data/` as downloaded, or should `data/EXEMPT.md` record an exemption? The availability cell's first sentence is to be updated once answered.

### 2026-11-21

Loaded the expert annotations (11,227 particles; 1,454 gold, 758 silica across AFM, SEM, TEM and TSEM). The loader also keeps each particle's centroid (nm). Added "Problems found on loading" with six checks, none of which removes rows:
- missing values: only `afm_height_nm` on non-AFM rows, which is structural;
- pixel size: fixed for AFM (5.87 nm/px), two values per material for SEM (gold 1.40–1.84, silica 0.92–1.86 nm/px);
- impossible values: none (no size ≤ 0, no min Feret > max Feret);
- group sizes: silica AFM has only 42 particles in 10 images (SEM 287, TEM 353);
- duplicates: 97 rows in scope have the same image and identical sizes, but 0 have the same centroid, so no particle is counted twice (sizes are whole-pixel multiples);
- format quirks: 4 lab "name" entries and 554 per-image "total" entries skipped.

Decisions logged in `decisions-log.md`:
- D1: SEM (BAM + LNE) is the primary reference; TEM is a secondary reference (AFM − TEM reported, min Feret only); TSEM is context only.
- D2: minimum Feret is the primary SEM measure; maximum Feret is a sensitivity check; ECD is not used.
- D3: duplicate check via centroids.

Descriptive statistics (median, IQR in nm): silica AFM 43.7 (3.0, n = 42), SEM 47.7 (2.7, n = 287), TEM 49.6 (4.4, n = 353). Gold medians pooled over both populations are not comparable: SEM BAM 64.3 (IQR 6.2) annotated almost only the 60 nm population, SEM LNE 34.2.

Histogram figure (probability density, bins of 4 nm for gold and 2 nm for silica): gold shows two modes near 30 and 60 nm, with few particles between 38 and 50 nm (AFM 25 of 620, SEM 5 of 318, TEM 8 of 422). Silica AFM peaks about 4 nm below SEM and TEM. Silica TEM has a tail at 55–65 nm (69 of 353).

Outlier screening (Tukey fences, 1.5 × IQR per group, nothing removed): silica TEM 41 of 353 flagged, silica SEM 11 of 287, silica AFM 1 of 42, silica TSEM 1 of 76; gold (pooled fences) 2 of 620 AFM.

Checks:
- Silica mean ≈ median (AFM 43.7 vs 43.7, SEM 47.7 vs 47.7), so "skewed" does not hold as the reason for the median for silica; revisit in task 005.
- Expert `afm_height` is not defined in any source found (Zenodo records, D5 report, validation report); "maximum height" is an assumption to test in task 006.

Added a final cell that prints versions (Python 3.14.6, numpy 2.5.3, pandas 3.0.6, matplotlib 3.11.2).

## Outcome

`project11.ipynb` loads the expert annotations for gold and silica and reports six loading checks with counts (no rows removed, no double-counted particles) plus an outlier screen. It has a descriptive-statistics table by material × technique (with per-lab SEM rows) and a histogram figure, each followed by a sentence giving the numbers. First look for silica: median AFM height 43.7 nm (n = 42) vs SEM minimum Feret 47.7 nm (n = 287); whether this difference exceeds the uncertainty is task 005. Carried to task 005: the outlier removal rule (silica TEM tail first), the gold population split, and the reason for the median. Still open: the mentor question on committing the data zips.
