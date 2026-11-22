# Decisions log

A brief record of each analytical decision: the question, the options considered, and the data facts that bear on it. Each entry names the option chosen. The reasoning behind each choice is written by the student in `judgment-log.md`, not here.

Entries are added when the student says to log a decision. Status is **open** until a choice is made.

---

## D1. Which electron-microscopy data count as "SEM" in the AFM comparison?

- **Date opened:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** The annotations include SEM (labs BAM and LNE), TSEM (BAM only) and TEM (CEA only). The question compares AFM with "SEM and SAXS". This decision fixes which rows form the SEM reference for nPSize01 (gold) and nPSize10 (silica). It must be made before the AFM vs SEM result is seen.

**Data facts (minimum Feret diameter, median, nm):**

| | SEM BAM | SEM LNE | TSEM BAM | TEM CEA | AFM VSL (n) |
|---|---|---|---|---|---|
| Gold (01) | 64 (n = 90) | 34 (n = 228) | 53 (n = 94) | 37 (n = 422) | n = 620, 20 images |
| Silica (10) | 47 (n = 96) | 48 (n = 191) | 45 (n = 76) | 50 (n = 353) | n = 42, 10 images |

- BAM's SEM and TSEM image numbers alternate (e.g. `2103076` SEM, `2103077` TSEM), which suggests that the same fields were recorded with both detectors. Not yet confirmed by comparing centroids.
- Pixel size: SEM 0.9–1.9 nm/px, TEM 0.26–0.40 nm/px.

**Options considered:**

- **A. SEM only (BAM + LNE).** Matches the question's wording and is the largest independent set. Pools two labs, which disagree for gold (64 vs 34 nm), so the result relies on the gold population split.
- **B. SEM + TSEM pooled.** More rows, and both measure the projected outline. Likely double-counts the same particles, so bootstrap CIs would be falsely narrow. *Set aside.*
- **C. SEM as the comparison, with TSEM and TEM in the stats table as context only.** Same comparison as A, plus an independent check of the SEM value (silica agrees within about 3 nm; for gold, TEM sits near LNE). Limitation: SEM and TEM both image dry particles in vacuum, so TEM cannot separate dry-state effects from AFM effects; only SAXS (in suspension) probes that. Requires fixing in advance what happens if TEM disagrees with SEM.
- **D. SEM from one lab only.** Removes the lab difference but leaves fewer particles, and the choice of lab could look like cherry-picking. *Set aside.*

**Decision:** Option C. SEM (BAM + LNE) is the reference for the AFM comparison. TSEM and TEM appear in the descriptive-statistics table as context only.

**Follow-up (decided 2026-11-21): what if TEM disagrees with SEM?**

- *Fact:* for gold, TEM (median minimum Feret 37 nm) and pooled SEM (57 nm) differ before the 30/60 nm split, mostly because the labs annotated different mixes of the two populations. TEM vs SEM for gold is therefore compared within each population after the split.
- *Options considered:*
  - **1. Report it as a limitation.** Keep SEM as the reference. The simplest option, but it leaves the doubt unresolved.
  - **2. Downgrade the conclusion.** Report that material's AFM vs SEM result as inconclusive and let SAXS decide. Honest, but SAXS is not in the draft scope.
  - **3. Always report AFM vs TEM as a sensitivity check.** Same structure as D2, but it extends D1 and adds comparisons.
- *Decision:* **Option 3.** AFM − TEM is reported next to AFM − SEM for each material, using the same criterion: |Δ| > K_COVERAGE · u_combined and the bootstrap 95% CI of the difference. The conclusion is called robust only if both comparisons point the same way. This extends D1: TEM is no longer context only, but a secondary reference. SEM (BAM + LNE) stays the primary reference. TSEM stays context only. The AFM − TEM check uses **minimum Feret only** (not maximum Feret), to keep the number of comparisons down.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## D2. Which SEM size measure is compared with AFM height?

- **Date opened:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** AFM height is the AFM size (tip convolution widens lateral size but not height). This decision fixes which in-plane SEM diameter it is compared with, for both nPSize01 (gold) and nPSize10 (silica). It must be made before the AFM vs SEM result is seen.

**Data facts:**

| SEM (BAM + LNE), median, nm | Minimum Feret | Maximum Feret |
|---|---|---|
| Gold (01) | 56.9 | 60.6 |
| Silica (10) | 47.7 | 49.6 |

- The validation report tabulates EM results as D_minFeret and D_maxFeret, and mentions a surface-area-based diameter.
- The expert `afm_height` is not defined in the annotations Zenodo record, the VSL images record, the D5 algorithms report or the validation report. That it is the maximum height above the substrate is an assumption, which can be tested against the VSL height maps.
- D5 algorithms report (p. 6–7): the SEM edge threshold that recovers the true radius depends on the material (about 0.5 for silica, about 0.6 for gold at R = 30 nm) and, for silica, on particle size. SEM sizes may therefore be biased differently for silica and gold, whichever measure is used.

**Literature:**

- Crouzier et al., *Beilstein J. Nanotechnol.* 10, 1523 (2019), doi:10.3762/bjnano.10.150. Compares AFM height with SEM area-equivalent, minimum Feret and maximum Feret on the same silica particles, and reports AFM/SEM discrepancies.
- nPSize protocols paper, *Nanomaterials* 13, 993 (2023), doi:10.3390/nano13060993.
- JRC: a particle's external dimension can be represented by the minimum Feret diameter and/or the maximum inscribed circle diameter. Implementation guidance EUR 31452 (2023), JRC132102.
- ISO 19749:2021 (SEM sizing) defines measurands. The contents were not checked.

**Options considered:**

- **A. Minimum Feret.** Matches the validation report and is the least affected by agglomerates. Its bias works against the hypothesis (conservative).
- **B. Maximum Feret.** In the report, but the most sensitive to agglomerates and touching particles. Its bias works in favour of the hypothesis.
- **C. Area-equivalent diameter (ECD).** Uses the whole outline. Needs a loader change (shoelace area from the outline), and the report's tables have no direct counterpart.
- **E. One primary measure, with the others as a sensitivity check.** The approach of Crouzier 2019.

**Decision:** Option E.
- **Primary:** minimum Feret. Used for the headline AFM − SEM difference and the |Δ| > k·u_combined test.
- **Secondary:** maximum Feret, as a sensitivity check.
- **ECD:** not included. It is bounded above by maximum Feret and, for near-round particles, lies between the two Feret values. Revisit only if the histograms show clearly non-spherical particles, or in task 006.
- **If minimum and maximum Feret lead to different conclusions:** report the result as dependent on the measure. Do not add ECD afterwards to decide between them.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## D3. How are duplicate rows checked and shown in the notebook?

- **Date opened:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** On loading, some annotation rows exactly match another row. The course asks that each loading problem be reported with a count and what was done. The question is whether the notebook itself shows that these are not double-counted particles.

**Data facts:**

- nPSize01 + nPSize10: 97 rows have the same image and identical sizes as another row (1,157 across all materials).
- 0 rows have the same image and the same centroid, so no particle is counted twice.
- Sizes are measured in whole pixels (e.g. SEM Feret values are multiples of 1.836 nm in BAM image `2103076.h5`), so identical sizes for different particles are expected.

**Options considered:**

- **1. Add the centroid to the loader and show the check in the notebook.** A small edit to the loader cell (two columns), and the check can be rerun in the notebook.
- **2. Leave the loader unchanged and state the finding in markdown.** No code, but the notebook makes a claim it does not demonstrate.

**Decision:** Option 1. The loader keeps `centroid_x_nm` and `centroid_y_nm` (unit checked as nm). Check (5) in "Problems found on loading" reports both counts. No rows are removed.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## D4. How are outliers screened for silica, and is anything removed?

- **Date opened:** 2026-11-21
- **Status:** decided 2026-11-21 (flag and removal rule). The follow-up on the size of the SEM shift is **open**.
- **Context:** The draft milestone asks for outliers to be reported with a count and what was done. The first version (commit `2771b2c`) used Tukey fences on size (1.5 × IQR per material × technique group) and removed nothing. This decision replaces that screen for silica (nPSize10). Gold is screened after its 30/60 nm population split (task 005).

**Data facts:**

- *Tukey fences, first version:* silica TEM 41 of 353 flagged (40 above 59.3 nm), SEM 11 of 287, AFM 1 of 42 (36.3 nm; next lowest 40.1 nm), TSEM 1 of 76. The pooled gold fences run from about −16 to 103 nm and flag 2 of 620 AFM heights.
- *Fences vs pixel steps:* the silica SEM IQR is 2.7 nm, so the fences sit about 2 LNE pixels (1.86 nm/px) beyond the quartiles.
- *Silica TEM tail is a session effect:* 68 of the 69 particles with minimum Feret > 55 nm are in the 10 CEA images from 2019. Median minimum Feret: 2019 = 57.6 nm, 2020 = 48.3 nm, 2021 = 49.4 nm. Median aspect ratio: 2019 = 1.12, 2020 = 1.03, 2021 = 1.03. The CEA images are not in `data/`.
- *Shape (aspect ratio = maximum Feret / minimum Feret):*

| Subset | Pixel size (nm/px) | n | p50 | p90 | p99 |
|---|---|---|---|---|---|
| SEM BAM | 0.92 | 96 | 1.06 | 1.27 | 1.41 |
| SEM LNE | 1.86 | 191 | 1.04 | 1.31 | 1.54 |
| TEM 2019 | 0.26 | 99 | 1.12 | 1.28 | 1.50 |
| TEM 2020 | 0.26 | 125 | 1.03 | 1.06 | 1.12 |
| TEM 2021 | 0.40 | 129 | 1.03 | 1.12 | 1.31 |

- *Pixel difference (maximum − minimum Feret, in pixels):* SEM LNE has 145 of 191 particles at 0–2 px, 2 each at 3 and 4 px, then a second group from 5 px. That gap is ≈ 5.6 nm, ≈ 1.12 for a 48 nm particle. SEM BAM declines smoothly with no gap.
- *Effect of the chosen flag (aspect ratio > 1.12):* SEM 72 of 287 flagged; the median falls from 47.74 to 46.52 nm (−1.21 nm). TEM 64 of 353 flagged; the median falls from 49.64 to 49.14 nm (−0.50 nm). Flagged SEM particles are larger: BAM median 48.7 nm flagged vs 46.8 nm unflagged; LNE 50.2 vs 46.5 nm.
- *For scale:* the silica AFM − SEM median difference is −4.0 nm, with a bootstrap 95% CI width of 2.2–2.5 nm (cell "Bootstrap unit").

**Options considered:**

- **A. Keep Tukey fences as a screen, nothing removed.** Meets the milestone as is. Assumes one population per group, cannot see the session-level TEM tail, and is sensitive to pixel steps.
- **B. A physical shape flag (aspect ratio), nothing removed, median shift reported.** Targets elongated or fused particles directly. Adds a cutoff that must be set from shape data only. Does not apply to AFM (tip-dilated lateral shape).
- **C. A shape flag with removal.** Changes the reference median.
- **D. A pixel-unit flag ((maximum − minimum Feret) / pixel size).** Scales with resolution. A gap exists for LNE only, and the pixel unit has no meaning for TEM (0.26 nm/px).
- **E. No screen, statement in markdown only.** The milestone wants a count from code.

**Decision:** Option B. `ASPECT_RATIO_FLAG = 1.12` in Parameters (the TEM 2020 99th percentile, matching the SEM LNE gap). The section "Particle shape and outlier check" shows the shape table by SEM lab and TEM session and the flag table. No particle is removed. Tukey fences are no longer used.

**Open follow-up:** the SEM median shifts by −1.21 nm when flagged particles are left out. Options to choose from: report it as a sensitivity result only; carry the "without flagged" median as a second SEM reference, like the maximum Feret check in D2; or fold it into the uncertainty budget. Separately open: whether the CEA 2019 TEM session counts as a valid measurement of nPSize10 (affects TEM as the secondary reference, D1).

**Reasoning:** see `judgment-log.md` (student's own entry).
