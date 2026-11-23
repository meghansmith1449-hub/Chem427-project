# 009: Part B of project11.ipynb, segment the silica AFM maps

- **Status:** done
- **Created:** 2026-11-21
- **Updated:** 2026-11-22
- **Due:** 2026-11-22

## Goal

Part B of `project11.ipynb` is the draft's first analysis: it segments the particles in the 10 silica (nPSize10) AFM height maps, takes each particle's maximum height, and reports the median size with a 95% image-bootstrap CI, shown in one figure beside the experts' heights and followed by a sentence giving the numbers.

## Progress log

### 2026-11-21

Found that the course's first analysis for the draft is segmentation of one material's AFM maps, not the A1–A2 comparison chosen in `plan.md`; moved Part B into the draft and kept A1–A2 as supporting work.

- B1: read the 10 silica maps from `data/VSL.zip` (2048-byte header, then 512 × 512 little-endian float64 heights in nm); substrate medians −0.17 to −0.08 nm, maximum heights 43.9 to 52.5 nm.
- B2: plane fit to pixels below `SUBSTRATE_MAX_NM` = 5 nm (tilt under 0.1 nm across a map), substrate noise σ = 0.10 to 0.12 nm, threshold 5 σ. 531 objects above threshold; whole particles cover 210 to 291 px at 36 to 47 nm, everything else is under 131 px and under 14 nm, so `MIN_PARTICLE_AREA_PX` = 170 cuts on area, not height. 39 particles kept, 6 large ones dropped at the edge. Counts per map match the experts in 8 of 10 maps (4/5 and 3/5 where the experts kept edge particles).
- B3: median 43.72 nm, 95% CI 43.19 to 44.45 nm (39 particles, 10 maps), against the experts' 43.72 nm, 95% CI 43.14 to 44.31 nm (42 particles). 22 of my 39 heights lie within 0.05 nm of an expert height.

To do: B2 and B3 captions, Next steps.

### 2026-11-22

- Added the property of the data that chose the method to the B2 cell: σ = 0.10 to 0.12 nm against particle heights of about 44 nm, roughly 400 σ.
- Wrote the Figure 3 caption (39 particles kept, separated from noise and debris by the area cut) and the Figure 4 caption (median 43.72 nm, 95% CI 43.19 to 44.45 nm, n = 39, against the experts' 43.72 nm, 43.14 to 44.31 nm, n = 42).
- Wrote Next steps: these data cannot say why AFM reads silica smaller, and the assumption checked next is that the experts' `afm_height` is the same measure as my maximum height above the substrate plane, checked by matching particles by centroid (task 010).

## Outcome

Done. My segmentation of the 10 silica AFM maps keeps 39 particles with a median height of 43.72 nm (95% CI 43.19 to 44.45 nm), the same median as the experts' 42 particles; the 3-particle difference is in two maps where the experts kept particles at the map edge. The check of what the experts' height measures continues in task 010.
