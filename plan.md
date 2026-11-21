# Plan: CHEM 427 Project 11, AFM vs SEM vs SAXS nanoparticle size

## Context
The question (topic.ipynb) asks two things:
(a) Does AFM height read silica (nPSize10) smaller than SEM and SAXS do, while reading gold (nPSize01) correctly, by more than the combined uncertainty?
(b) Can the student's own segmentation reproduce the expert-annotated size distributions?
The student set six end-of-project goals. They match the final rubric: reproducibility, data description, method, result, judgment log, and workflow. A second list, the draft goals due 2026-11-22, is in the "Draft milestone" section below.

Current state: project11.ipynb has a title, an intro, and an empty Parameters heading. data/ holds the raw downloads: 4 image zips (LNE1-3, VSL), annotations.zip, SAXS .dat files and the validation report PDF. Task 004 is active. There is no .claude/skills/ yet.

## Plan by goal (words only, no code until the student says proceed)

### Goal 1: Reproducible from a clean start
- First cell: an availability statement saying where the data come from, the 5 DOIs, the licenses, and that the files are in data/ as downloaded.
- Every path is relative (data/...). Zips are read in place with zipfile and never extracted by hand.
- The Parameters section holds RNG_SEED and a seeded generator for the bootstrap.
- Check: Restart & Run All, plus a final cell that prints the Python and package versions.
- Flag: the zips add up to about 225 MB. Each is under GitHub's 100 MB limit, but confirm with the mentor that committing them is acceptable.

### Goal 2: The question and the data, described
- The question in one sentence, at the top.
- A source table: each Zenodo record with its URL and access date (2026-11-08, the download date).
- Units and the row unit: one row of the annotation table = one expert-annotated particle in one image (laboratory, technique, material, image, pixel size, Feret/height in nm). One SAXS row = q (nm⁻¹), I(q), and σ.
- The removal rule, written as a constant with its reason, e.g. particles touching the image edge, agglomerates, or heights below a noise floor. Report how many rows each filter removes.

### Goal 3: Method, the data property that chose it, and an assumption check
- AFM: subtract a plane or line background, threshold the height, and label connected regions. Size = maximum height per particle. Height is chosen because tip convolution widens lateral size but not height.
- SEM: use the annotated area-equivalent or Feret diameter. Optionally, segment it yourself for part (b).
- SAXS: fit a sphere form factor with a size distribution, or use the first minimum (qR = 4.493) as a quick estimate. Compare with the validation-report value.
- The median and a bootstrap CI were chosen because the distributions may be skewed or (for nPSize01) bimodal. For nPSize01, split the 30 nm and 60 nm populations before taking medians.
- Agreement with the experts (part b): compare distributions with a two-sample KS test, a Q-Q plot, or median differences on the same images.
- Assumptions to check, with the result reported: background flatness (plot the residual substrate height), segmentation threshold sensitivity (rerun at ±the chosen value), sphere shape for SAXS (fit residuals).

### Goal 4: Main result with uncertainty, read against the chemistry
- A table per material: AFM, SEM and SAXS sizes in nm, each ± uncertainty, plus the differences and combined uncertainty (root-sum-square of the independent u's).
- Is the silica difference larger than k·u_combined? Is the gold difference not?
- Read the result against the chemistry. A plausible size is ~50 nm silica and ~30/60 nm gold. A silica AFM deficit fits a compressible or hydrated shell, tip load, or substrate deformation. State which, and what you can't rule out.
- Limits: one AFM instrument/lab subset, threshold choice, the SAXS model.

### Goal 5: Judgment log (student only)
- The student writes every entry and the AI-use paragraph. The assistant will not draft, polish or supply text for judgment-log.md (course rule).
- What the assistant can do: point to the decisions worth logging (threshold, removal rule, nPSize01 population split, SAXS model), and say afterward whether a draft cites a specific number from the output.
- Two entries must be dated after 2026-11-22.

### Goal 6: Workflow
- AGENTS.md: the student adds project-specific steering, e.g. the annotation JSON structure, which zip holds which material/technique, units, and corrections given twice.
- Skill: the repeated step is "segment one AFM height map → table of particle heights". Put it in .claude/skills/<name>/SKILL.md, use it on both materials, and show its output in the notebook.
- tasks/: close 004 (loading + exploration) and 005 (first analysis) for the draft, then open 006+ (e.g. SEM comparison, SAXS fit, expert-vs-own comparison, final write-up). Each gets dated progress entries with numbers.

## Decisions from the interview (2026-11-20)
- **Draft data:** the expert annotations only. Your own AFM segmentation is postponed to task 006.
- **Draft materials:** both silica (nPSize10) and gold (nPSize01).
- **Outliers:** decided after looking at the histograms. Record the choice and the reason in the judgment log.
- **Gold split:** undecided. Look at the histogram first, then choose between a fixed cutoff and a Gaussian mixture; the assistant explains the trade-offs at that point.
- **Criterion:** report both the bootstrap 95% CI of the AFM−SEM median difference and |Δ| > k·u_combined with K_COVERAGE = 2. Both are fixed in Parameters before the result is seen.
- **Uncertainty budget:** undecided. Once the bootstrap CI widths are known, compare them with the report's instrument terms, then decide.
- **Data commit:** ask the mentor whether to commit the zips (~225 MB) as downloaded or write data/EXEMPT.md. This goes in as a task item.
- **Tasks:** rescope 004 to "load and explore the annotations" and add 005 "first analysis: AFM vs SEM median + bootstrap". Segmentation becomes 006.

## Draft milestone: due 2026-11-22 (two days from now)
Scope it narrow. Use the expert annotations, which already load in topic.ipynb. Leave your own segmentation and SAXS until after the draft.

**Data**
- Move the annotation-loading cell from topic.ipynb into project11.ipynb (relative path data/annotations.zip). Add the sources and access date, plus the row description: one annotated particle, size in nm.
- Problems to look for on loading, each reported with a count and what was done:
  - missing measurement fields (columns that are NaN for some labs or techniques);
  - pixel_size varies between images;
  - outliers (very small or very large diameters, e.g. agglomerates or segmentation fragments);
  - format quirks (non-"particle" keys, nested measurements).
- Each removal rule is a named constant in Parameters, with its reason.

**Exploration**
- A table of descriptive statistics (n, median, IQR, mean, SD) grouped by material (nPSize01, nPSize10) × technique (AFM, SEM).
- One figure: size histograms per material, with AFM and SEM as different line styles or markers. For nPSize01, expect two modes at about 30 and 60 nm.
- After each output, one sentence giving the numbers (e.g. "silica AFM median X nm vs SEM Y nm, n = …").

**First analysis**
- Method: median with a bootstrap 95% CI (seeded generator), AFM vs SEM, for nPSize10. For nPSize01, the 30 and 60 nm populations are split by a cutoff constant.
- The data property that chose the method: the distributions are skewed and (gold) bimodal, so a median with a bootstrap CI is used instead of a mean ± SD.
- First result: the silica AFM − SEM difference in nm with its CI, and the same for gold.
- Tasks: 004 is rescoped to loading + exploration; 005 holds this first analysis (see Decisions).

**Judgment log (student writes, ≥2 entries)**
- Decisions this draft produces that are worth an entry: the outlier/removal cutoff, the gold population cutoff, and the choice of median over mean. Each entry cites the number from the output.

**Workflow**
- AGENTS.md: the student adds their own lines about the data. Examples: which zips hold which lab or technique, the structure of the annotation JSON, that AFM size = height in nm.
- tasks/: 001–003 are already closed ✓. You need ≥2 tasks of your own with dated entries: 004, plus a new 005 (e.g. "Exploration and loading problems"), both with entries dated 11-20 to 11-22 that include numbers.

## Suggested order (after the draft)
1. Goals 1–2 skeleton (availability, sources, Parameters, loading, row description), done in tasks 004–005 for the draft.
2. Task 006: AFM nPSize10 segmentation + median/bootstrap, compared with the experts.
3. Repeat for nPSize01 (bimodal split) → make this the skill.
4. SEM from annotations, then the SAXS fit.
5. The uncertainty comparison table and interpretation (Goal 4).
6. Assumption checks, then a clean rerun.
Throughout: student judgment-log entries and task updates.

## Verification
- Kernel restart → Run All with no errors, and the versions cell prints.
- The gold AFM median is close to the report's reference values (sanity check that the pipeline is correct).
- The row counts before and after each filter are printed.
