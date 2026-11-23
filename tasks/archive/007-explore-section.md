# 007: Explore section of project11.ipynb

- **Status:** done
- **Created:** 2026-11-21
- **Updated:** 2026-11-22
- **Due:** 2026-11-22

## Goal

The "Explore" section of `project11.ipynb` gives each particle one size (AFM height or SEM minimum Feret, nm), shows a statistics table (n, median, IQR, mean) for AFM and SEM by material, and a histogram figure with AFM and SEM per material. Each output is followed by a sentence giving its numbers, and outliers are decided from the figure.

## Progress log

### 2026-11-22

- Each particle has one size, `size_nm`: AFM height for AFM, minimum Feret for SEM (N1).
- Table 1 gives n, median, IQR and mean for 11 groups. Silica: AFM 43.7 nm (n = 42), SEM 47.7 nm (n = 287), BAM and LNE within 1.0 nm. Gold 60 nm: BAM 64.3 nm against LNE 60.0 nm, a 4.3 nm lab difference, larger than the AFM − SEM difference.
- Figure 1 showed gold is bimodal (peaks near 30 and 60 nm for both techniques), so gold is split at `GOLD_SPLIT_NM` = 45 nm (N3). Pooled gold medians were 37.9 nm (AFM) and 56.9 nm (SEM); split, 28.7 / 30.7 nm (30 nm) and 58.7 / 61.4 nm (60 nm). Noted after the Figure 1 caption as the change to the plan.
- Outlier check: every group's median is unchanged to 0.1 nm with 1% trimmed at each end; nothing removed (N6).

## Outcome

Done. The Explore section has Table 1 and Figure 1, each followed by a sentence giving its numbers, the outlier check, and the note on what the exploration changed (the gold split).
