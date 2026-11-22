# Plan: CHEM 427 Project 11, nanoparticle size by three techniques

The notebook follows the question in order. The previous plan is in git history (commit `6a278f0`).

## The question, in parts

1. Does AFM height read **silica** (nPSize10) smaller than **SEM**?
2. …and smaller than **SAXS**?
3. Does AFM read **gold** (nPSize01, 30 and 60 nm populations) correctly?
4. Are those differences larger than the **combined uncertainty**?
5. Can **my own segmentation** of the AFM maps reproduce the experts' size distributions?

## Notebook outline

1. **Question and data**: question, sources, availability. *(done)*
2. **Parameters**: every scientific choice, with unit and reason. *(started)*
3. **Load**: expert annotations, plus a numbered list of problems found and what was done.
4. **Explore**: size histograms by technique and material, and a small statistics table.
5. **Part A: AFM vs SEM and SAXS** (question parts 1–4)
   - A1. Silica: AFM − SEM median difference with an image-bootstrap CI (figure).
   - A2. Gold: the same, per population (split at `GOLD_SPLIT_NM`).
   - A3. SAXS: the report's SAXS size for each material, compared with AFM.
   - A4. Combined uncertainty: sampling (bootstrap) plus calibration terms, √(u₁² + u₂²); is |Δ| larger?
6. **Part B: my own segmentation** (question part 5)
   - B1. Load the AFM height maps (2048-byte header, 512 × 512 heights in nm).
   - B2. Flatten the background (S11) and segment above the noise (S12).
   - B3. Each particle's maximum height; median with an image-bootstrap CI.
   - B4. Compare with the experts' AFM heights for the same material.
7. **Conclusion**: the answer to each part, with uncertainty and limits.
8. **Environment**.

## Course requirements, mapped

| Requirement | Where |
|---|---|
| First analysis (draft) | Part A1–A2 (decided 2026-11-21) |
| One more method | Part B segmentation (S11, S12) |
| One named check | A3: the report's SAXS and reference values |
| Skill | B2–B3 as "segment one map → table of particle heights", used on silica and gold |
| Visual summary (≤ 3 figures) | A1–A2 difference figure, B4 comparison figure |

## Decided 2026-11-21: what the draft (due 2026-11-22) contains

- **Option 1: Part A1–A2. Chosen.** First in question order. Already computed and checked in the archived notebook, so it is quick to rebuild.
- **Option 2: Part B on silica.** The course's suggested first analysis (project list). It is new work.

## Style

- Figures rather than wide tables; about 25 cells for the final.

## Decided for the new notebook

- N1: SEM size measure is minimum Feret.
- N2: SEM reference is BAM + LNE pooled, images resampled within each laboratory.
- N3: gold split by a fixed cutoff, `GOLD_SPLIT_NM = 45` nm (midpoint of the nominal sizes).
- N4: techniques are AFM and SEM only (to confirm with the mentor).
- N5: bootstrap unit is images, with the particle bootstrap shown beside it as the named alternative.
- N6: no outliers removed; the notebook shows the medians are unchanged without the extremes.
