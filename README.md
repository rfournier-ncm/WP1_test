# Face Familiarity — Evidence Accumulation Modelling

**Authors:** Raphaël Fournier, Paul Downing & Richard Ramsey

**Preprint:** [link]

**Status:** [e.g., under review]

---

## Overview

This project contains the analysis code and manuscript for a four-experiment study examining familiarity effects in face recognition using evidence accumulation modelling (LBA). Each experiment is self-contained in its own folder but follows a shared structure and file naming convention (see below).

| Experiment | Design | N |
|-----------|--------|---|
| Exp 1 | [brief description] | 31 |
| Exp 2 | [brief description] | 35 |
| Exp 3 | [brief description — note: LBA not fitted due to quantised RTs] | 40 |
| Exp 4 | [brief description] | 46 |
| Exp 5 | [brief description] | 46 |
---

## Workflow components

- [renv](https://rstudio.github.io/renv/articles/renv.html) for R package version management
- [git](https://git-scm.com/) / [GitHub](https://github.com/) for version control and sharing
- [Tidyverse](https://www.tidyverse.org/) for data wrangling and visualisation
- [EMC2](https://github.com/ampl-psych/EMC2) for LBA evidence accumulation modelling
- [Quarto](https://quarto.org/) for reproducible scripts and manuscript rendering
- [preprint-typst](https://github.com/mvuorre/preprint) Quarto extension for manuscript formatting

---

## Getting started

1. Clone or download the repository to your local machine.
2. Open `face-familiarity-eam.Rproj` — renv will automatically bootstrap itself.
3. Run `renv::restore()` to install all packages at the recorded versions.
4. You can now run any of the `.qmd` scripts within each experiment folder.

> **Note on parallel model fitting (macOS Apple Silicon):** R 4.x on macOS aarch64 defaults to Apple's vecLib BLAS, which breaks `mclapply`-based parallelism used by EMC2. To enable parallel fitting, switch to standard BLAS in terminal:
> ```bash
> sudo ln -sf libRblas.0.dylib libRblas.dylib
> ```
> (in `/Library/Frameworks/R.framework/Resources/lib/`). To revert: `sudo ln -sf libRblas.vecLib.dylib libRblas.dylib`. Alternatively, set `cores_per_chain = 1` (slow but works without the fix).

---

## Data availability

Raw data and fitted model objects are available via this GitHub repo (link) or via the OSF (link).
Large files are posted on the OSF and can be downloaded directly via the scripts below whenever necessary.

The processed data files (e.g., `data_exp_N.csv`) are included and are sufficient to reproduce all modelling and effects analyses.

---

## Repository structure

```
face-familiarity-eam/
├── exp_1/                   # Experiment 1
├── exp_2/                   # Experiment 2
├── exp_3/                   # Experiment 3
├── exp_4/                   # Experiment 4
├── manuscript/              # Quarto manuscript and supplementary
│   ├── manuscript.qmd
│   ├── supplementary.qmd
│   ├── bibliography.bib
│   └── figures/
├── _extensions/             # preprint-typst Quarto extension
├── _freeze/                 # Quarto freeze cache (manuscript)
├── docs/                    # Rendered manuscript output
├── renv/                    # renv infrastructure
├── renv.lock                # Package version lockfile
├── _quarto.yml              # Project-level Quarto config
└── face-familiarity-eam.Rproj
```

---

## Shared experiment structure

Each experiment folder (`exp_1/` through `exp_4/`) follows the same internal layout and file naming convention, with `N` replaced by the experiment number.

### Scripts (`.qmd` files)

| File | Purpose |
|------|---------|
| `wrangle.qmd` | Reads raw data, applies exclusions, wrangles into analysis-ready format |
| `model.qmd` | Fits the confirmatory LBA model(s) using EMC2 |
| `model_exploratory.qmd` | Fits exploratory model variants (bias, full, t0 parameterisations) |
| `effects.qmd` | Extracts and visualises confirmatory parameter estimates and posterior predictives |
| `effects_exploratory.qmd` | Extracts and visualises exploratory model results |
| `parameter_recovery.qmd` | Parameter recovery validation (Exp 1 only) |

### Data files (`data/`)

| File | Contents |
|------|---------|
| `raw_data_exp_N.csv` | Concatenated raw trial-level data |
| `data_exp_N.csv` | Processed, analysis-ready data |
| `demo_exp_N.csv` | Raw demographic data |
| `demo_exp_N_clean.csv` | Cleaned demographic data |
| `exclusions_exp_N.csv` | Participant exclusion log |
| `fam_stim_exp_N.csv` | Familiarity stimulus information (where applicable) |
| `wrangle_data_exp_N.csv` | Intermediate wrangling output |
| `raw/` | Per-participant raw data files (subdirectory) |
| `sbc/` | Simulation-based calibration outputs (Exp 1 only) |

### Model objects (`models/`)

Model objects are not publicly shared (see Data availability above), but would be placed here if obtained from the authors. Naming convention:

| File | Contents |
|------|---------|
| `LBA_expN_v1.RData` | Confirmatory LBA model fit |
| `LBA_expN_v1_full.RData` | Exploratory: full parameterisation |
| `LBA_expN_v1_bias.RData` | Exploratory: bias parameterisation |
| `LBA_expN_v1_t0.RData` | Exploratory: t0 parameterisation |
| `pp_LBA_expN_v1*.RData` | Posterior predictive samples for each model variant |

### Figures (`figures/`)

| File | Contents |
|------|---------|
| `rt_acc.jpeg` | Descriptive RT and accuracy plot |
| `drift_rate.jpeg` | Drift rate parameter estimates |
| `threshold.jpeg` | Threshold parameter estimates |
| `all_params.jpeg` | All LBA parameters combined |
| `pp_cdf.png` | Posterior predictive CDF |
| `pp_density.png` | Posterior predictive density |
| `chains.png` | MCMC chain diagnostics |
| `pars.png` | Parameter summary plot |
| `exploratory/` | Corresponding plots for all exploratory model variants |

---

## Manuscript

The manuscript is written in Quarto (`manuscript/manuscript.qmd`) using the [preprint-typst](https://github.com/mvuorre/preprint) extension. Supplementary materials are in `manuscript/supplementary.qmd`. References are managed via `manuscript/bibliography.bib`.

Rendered outputs are written to `docs/manuscript/` and can be viewed without re-running the analyses.

To render the manuscript locally:

```bash
quarto render manuscript/manuscript.qmd
```

---

## How to reproduce the full workflow

For each experiment, run scripts in this order:

1. `wrangle.qmd` — process raw data
2. `model.qmd` — fit confirmatory LBA model *(slow; run chunk by chunk)*
3. `model_exploratory.qmd` — fit exploratory variants *(slow; run chunk by chunk)*
4. `effects.qmd` — extract estimates and generate figures
5. `effects_exploratory.qmd` — extract exploratory estimates and figures

Then render the manuscript:

6. `quarto render manuscript/manuscript.qmd`

> Model fitting scripts are not designed to be run end-to-end in one go due to long runtimes. Run them chunk by chunk.
