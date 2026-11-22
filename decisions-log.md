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

**Amendment (2026-11-21), scope of the draft:** TSEM is removed from the notebook, and the AFM − TEM check (follow-up option 3) is dropped from the draft. TEM stays in the descriptive-statistics table as context. Either can return for the final submission. Context: the course's final-project page asks for one named comparison or check, and puts a finished notebook at about 15 to 25 cells.

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

**Amendment (2026-11-21), scope of the draft:** the maximum Feret sensitivity check is dropped from the draft. Minimum Feret stays the only SEM measure. It can return for the final submission.

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

**Decision:** Option B. `ASPECT_RATIO_FLAG = 1.12` in Parameters (the TEM 2020 99th percentile, matching the SEM LNE gap). The section "Checks on the result" prints the TEM 2020 99th percentile, the TEM medians by session and the flag table. The full shape table by SEM lab and TEM session is in commit `c45401b` (Appendix A3). No particle is removed. Tukey fences are no longer used.

**Open follow-up:** the SEM median shifts by −1.21 nm when flagged particles are left out. Options to choose from: report it as a sensitivity result only; carry the "without flagged" median as a second SEM reference, like the maximum Feret check in D2; or fold it into the uncertainty budget. Separately open: whether the CEA 2019 TEM session counts as a valid measurement of nPSize10 (affects TEM as the secondary reference, D1).

**Reasoning:** see `judgment-log.md` (student's own entry).

---
---

# Restart: new notebook (2026-11-21)

`project11.ipynb` was restarted in question order (old version: `archive/project11-2026-11-21.ipynb`). Entries D1–D4 above belong to the old notebook and are kept only as history. They do not carry over. Every decision for the new notebook is made again below, against the current goals in `plan.md`, starting from N1.

---

## N1. Which SEM size measure is compared with AFM height?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** AFM size is the particle's height. SEM sees the projected outline, and this fixes which diameter from it is compared with AFM height, for both materials.

**Data facts (SEM, BAM + LNE, median):**

| | Minimum Feret | Area-equivalent diameter | Difference |
|---|---|---|---|
| Silica (n = 287) | 47.74 nm | 47.03 nm | −0.71 nm |
| Gold 30 nm (n = 122) | 30.71 nm | 30.34 nm | −0.37 nm |
| Gold 60 nm (n = 196) | 61.42 nm | 62.49 nm | +1.07 nm |

- Area-equivalent diameter computed from each particle's outline (shoelace area, in pixels × pixel size); not in the notebook.
- Every difference is within about one SEM pixel (0.92–1.86 nm/px).
- The validation report tabulates electron-microscopy sizes as minimum and maximum Feret.
- The course project description says SEM "measures a projected area".

**Options considered:** minimum Feret (loaded); maximum Feret (loaded, most affected by touching or elongated particles); area-equivalent diameter (needs the outline area added to the loader).

**Decision:** minimum Feret.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N2. Which SEM measurements form the SEM reference?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** Two laboratories, BAM and LNE, annotated SEM particles. This fixes which of them form the SEM value that AFM is compared with, for silica and both gold populations. Decided before the AFM − SEM result of the new notebook is computed.

**Data facts (SEM minimum Feret, median; gold split at 44 nm):**

| | BAM | LNE |
|---|---|---|
| Silica | 46.86 nm (n = 96, 6 images) | 47.92 nm (n = 191, 7 images) |
| Gold 30 nm | none | 30.71 nm (n = 122, 10 images) |
| Gold 60 nm | 64.26 nm (n = 90, 6 images) | 60.03 nm (n = 106, 10 images) |

- A bootstrap that resamples images within each laboratory keeps the labs' shares fixed, so the gap between labs moves the pooled median but adds little to the CI width. The gap enters the uncertainty only if it is added as its own term in the combined uncertainty (A4), which is a later decision.

**Options considered:**
- **A. Pool BAM + LNE**, images resampled within each laboratory.
- **B. LNE only**, the one lab with all three populations.
- **C. Each lab separately against AFM.**

**Decision:** A. Pool BAM + LNE, resampled within each laboratory. Gold 30 nm is LNE only, because BAM has no particles there.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N3. How are the gold 30 nm and 60 nm populations separated?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** nPSize01 is a mix of about 30 nm and 60 nm gold spheres, and the labs annotated different shares of each (BAM almost only the 60 nm population). A median over the mix reflects those shares, not the technique, so each population is compared on its own. Decided before the AFM − SEM result of the new notebook is computed.

**Data facts (gold, primary size: AFM height, SEM minimum Feret):**

- Both techniques show a near-empty gap between about 40 and 48 nm (Explore figure). From 42 to 48 nm: SEM 0 particles, AFM 8.
- Moving the cutoff anywhere from 42 to 46 nm changes the group of 6 of 938 particles (620 AFM + 318 SEM).
- Course precedent: a cutoff is a named constant in Parameters (S02); a threshold is set from a property of the data and its sensitivity shown (S12). No session teaches mixture models.

**Options considered:**
- **A. A fixed cutoff in the gap**: 44 nm (middle of the observed gap) or 45 nm (midpoint of the nominal sizes, (30 + 60) / 2).
- **B. A two-Gaussian mixture**, each particle assigned to the more likely population.

**Decision:** A, at 45 nm. `GOLD_SPLIT_NM = 45` nm: below is the 30 nm population, at or above the 60 nm population.

**Sensitivity around 45 nm:** moving the cutoff to 44 nm moves 2 of 938 particles (44.5 and 44.6 nm) into the 60 nm population; moving it to 46 nm moves 1 (45.7 nm) into the 30 nm population.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N4. Which techniques are compared with AFM?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21, **to confirm with the mentor**
- **Context:** The annotations hold four techniques: AFM, SEM, TEM and TSEM. This fixes which of them form the electron-microscopy side of the comparison.

**Data facts:**

- The question in `AGENTS.md` and the notebook says "SEM and SAXS"; the course project list says "electron microscopy and small-angle X-ray scattering", which would also cover TEM.
- TEM comes from one laboratory (CEA). Its silica median minimum Feret by session: 2019 = 57.6 nm, 2020 = 48.3 nm, 2021 = 49.4 nm.
- BAM's SEM and TSEM image numbers alternate (e.g. `2103076` SEM, `2103077` TSEM), which suggests the same fields recorded with two detectors; not confirmed.

**Options considered:**
- **A. AFM and SEM only.**
- **B. Add TEM as a second electron-microscopy reference.**
- **C. Pool SEM and TEM as "electron microscopy".**

**Decision:** A. `TECHNIQUES = ["AFM", "SEM"]`. Question for the mentor: does "electron microscopy" in the project description require TEM as well?

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N5. What does each bootstrap resample draw?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** The median AFM and SEM sizes and their difference get a bootstrap 95% CI (Part A). This fixes the resampling unit for silica and both gold populations.

**Data facts:**

- S14 notes: "Bootstrap over the unit that is independent… Draw whole the unit that errs together." Also: with few groups, "the spread is itself poorly known".
- Particles in one image share the AFM tip or SEM beam, the calibration and the background.
- Silica AFM has 42 particles in 10 images; gold AFM 620 in 20; SEM 6 to 16 images per lab and population.
- Old notebook (silica AFM − SEM, before the restart): 95% CI −4.78 to −2.68 nm by image, −5.03 to −2.47 nm by particle.
- Course final-project page, Method "for a 5": compare the method with a named alternative on your data, with a number.

**Options considered:**
- **A. Images.**
- **B. Particles.**
- **C. Images as the result, with particles shown beside it as a named alternative.**

**Decision:** C. `BOOTSTRAP_UNIT = "image"`; the particle bootstrap is reported next to it.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N6. Are outliers removed?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** The analysis compares medians of AFM height and SEM minimum Feret per population (silica, gold 30 nm, gold 60 nm, split at 45 nm). This fixes whether extreme particles are removed first.

**Data facts (primary size):**

| Group | Median | Most extreme values |
|---|---|---|
| Gold 30 nm AFM (n = 324) | 28.7 nm | low: 11.4, 11.5, 13.0 nm |
| Gold 60 nm AFM (n = 296) | 58.7 nm | high: 103.2, 105.2 nm (next 77.9 nm) |
| Gold 30 / 60 nm SEM (n = 122 / 196) | 30.7 / 61.4 nm | 22.1–41.9 / 48.9–82.6 nm |
| Silica AFM (n = 42) | 43.7 nm | low: 36.3 nm (next 40.1 nm) |
| Silica SEM (n = 287) | 47.7 nm | high: 57.7, 60.6 nm |

- Dropping the most extreme 1% at each end changes no median at the 0.1 nm level.
- Heights of 103–105 nm on 60 nm gold are consistent with one particle on top of another; 11–13 nm on 30 nm gold could be fragments.

**Options considered:**
- **A. Keep all particles**, and show that the medians do not change when the extremes are dropped.
- **B. Remove by a rule**, e.g. AFM heights above a limit as stacked particles.

**Decision:** A. No particle is removed.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N7. What is the draft's first analysis, and on which material?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** The course's project description says: "A first analysis for the draft. Segment the particles in the AFM maps of one material, take each particle's maximum height, and report the median size with a bootstrap confidence interval." `plan.md` (decided earlier on 2026-11-21) had chosen Part A1–A2, the AFM − SEM comparison on expert annotations, as the draft's first analysis, and listed Part B on silica as "the course's suggested first analysis".

**Data facts:**

- `data/VSL.zip` holds 10 silica AFM maps (`vsl_lne_npsize_10_…`) and 20 gold maps (`vsl_lne_npsize_01_…` and `vsl_lgc_npsize1_…`).
- Expert annotations on those maps: silica 42 particles in 10 maps; gold 620 particles in 20 maps (324 in the 30 nm population, 296 in the 60 nm population).
- Silica is one population; gold needs the 45 nm split (N3) before a median means anything.
- Expert silica AFM median: 43.7 nm (Table 1), a value to check a segmentation against.

**Options considered:**
- **A. Keep A1–A2 as the draft's first analysis.** Already built; does not do what the course's wording describes.
- **B. Part B on silica.** One population; the material the question centres on; few particles (42 annotated).
- **C. Part B on gold.** More particles; needs the population split first.

**Decision:** B. Part B (B1–B3) on silica is the draft's first analysis. A1–A2 stays in the notebook as Part A. Whether the course requires or only suggests this analysis is not yet confirmed with the mentor.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N8. How are the AFM maps flattened, and what counts as above the noise?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** Part B segments particles as connected regions above the substrate. This fixes how the substrate is levelled and how far above it a pixel must be to count. Set before the B3 median was computed.

**Data facts (10 silica maps):**

- Raw substrate medians −0.17 to −0.08 nm; maximum heights 43.9 to 52.5 nm.
- A plane fitted to pixels below 5 nm changes heights by at most 0.09 nm across a map: the maps arrive nearly flat.
- Substrate noise σ (from the MAD) 0.10 to 0.12 nm.
- Sensitivity, with everything else fixed (particles kept, median height):

| Setting | Particles | Median |
|---|---|---|
| Substrate below 5 nm, threshold 5 σ (chosen) | 39 | 43.72 nm |
| Substrate below 2 nm | 39 | 43.72 nm |
| Substrate below 10 nm | 39 | 43.72 nm |
| Threshold 3 σ | 49 | 43.08 nm |
| Threshold 8 σ | 39 | 43.72 nm |

- Course precedent: S11 fits a baseline only through anchor regions away from the peaks; S12 measures noise in a stretch with no peaks and tries prominence from 5 times the noise (`NOISE_SIGMAS = 5`).

**Options considered:**
- **Flattening:** none (maps already near 0 nm); a plane through substrate pixels; a higher-order surface.
- **Noise:** standard deviation of substrate pixels; the MAD (robust to particle skirts and debris below the substrate cut).
- **Threshold:** 3 σ, 5 σ, 8 σ.

**Decision:** a plane through pixels below `SUBSTRATE_MAX_NM = 5.0` nm; σ from the MAD; `THRESHOLD_SIGMA = 5`.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N9. Which segmented objects count as whole particles?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** At 5 σ, 531 objects are found in the 10 silica maps, most of them specks of noise or debris. This fixes which are kept as particles. The cutoff was set from the B2 area-against-height figure, after seeing the objects but before computing the B3 median.

**Data facts:**

- Whole particles inside the map: 210 to 291 px, 36 to 47 nm tall. Every other object: under 131 px and under 14 nm. Any area cutoff from 132 to 209 px keeps the same 39 particles.
- 6 large objects touch the map edge. Keeping them gives 45 particles and a median of 43.90 nm (vs 39 and 43.72 nm).
- Particles per map, mine / experts: 4/5, 6/6, 8/8, 4/4, 3/5, 3/3, 2/2, 4/4, 2/2, 3/3. The two shortfalls are maps where the experts kept particles at the edge.
- No object is much larger than 291 px, so no touching pair was found merged into one object.
- Course precedent: S12 keeps only peak apexes inside `KEEP_MIN`, clear of the run's two edges; S12 sets detection from the narrowest real peak's width, not from the quantity reported.

**Options considered:**
- **Size filter:** a minimum area (independent of the height being measured); a minimum height (separates the groups equally well here, but selects on the measured quantity).
- **Edge:** drop objects touching the map edge (a particle there may be cut off); keep them (matches the experts' counts in two maps).

**Decision:** keep objects of at least `MIN_PARTICLE_AREA_PX = 170` px that do not touch the map edge.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N10. What is a segmented particle's size?

- **Date:** 2026-11-21
- **Status:** decided 2026-11-21
- **Context:** The AFM height of a rigid sphere on a flat substrate is its diameter (course project description). This fixes how one height is read from each segmented particle.

**Data facts (39 silica particles, median):**

| Height read as | Median |
|---|---|
| Tallest pixel (chosen) | 43.72 nm |
| Mean of the 5 tallest pixels | 43.57 nm |
| Mean of the 9 tallest pixels | 43.44 nm |

- The tallest pixel carries about +1 σ of noise (σ ≈ 0.1 nm). Averaging more pixels pulls in points down the particle's curved top.
- 22 of the 39 heights lie within 0.05 nm of an expert height in the same maps; the expert heights come in steps of about 0.08 nm.
- The course project description says "take each particle's maximum height".

**Options considered:** the tallest pixel; the mean of the top few pixels; a fitted cap.

**Decision:** the tallest pixel above the flattened substrate.

**Reasoning:** see `judgment-log.md` (student's own entry).

---

## N11. Which SAXS curve is fitted: raw or background-subtracted?

- **Date opened:** 2026-11-21
- **Status:** **open**, for the final submission (A3)
- **Context:** `data/` holds 17 SAXS `.dat` files. For nPSize1 (gold) and nPSize10 (silica) there is a raw curve and a `_wo_background` curve. This fixes which is fitted. The comparison cells are set aside in `archive/saxs-background-check.ipynb`.

**Data facts:**

- Both versions share the same q grid (0.031 to 1.160 nm⁻¹, 737 points).
- Raw − subtracted is the same curve for gold and silica: about 184 cm⁻¹ at the lowest q, levelling near 5.2 cm⁻¹, falling to 3.6 cm⁻¹ at the highest q. It is a measured background, not a constant.
- At q ≈ 0.95 nm⁻¹ the background is 95% (gold) and 85% (silica) of the raw signal.
- `data/` has no blank or solvent measurement to subtract independently.
- All four files carry the same `HDF: fcm_2020kw28_00032` header line although their intensities differ.

**Options considered:** fit the raw curve with a background term; fit the `_wo_background` curve as provided. Separately: the upper q limit of the fit.

**Decision:** open.

**Reasoning:** see `judgment-log.md` (student's own entry).
