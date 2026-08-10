# Mathematical Modeling Study

A from-scratch, no-prior-knowledge-assumed study companion to:

> Michael Y. Li, **An Introduction to Mathematical Modeling of Infectious Diseases**, Springer, Mathematics of Planet Earth series, 2018. ISBN 978-3-319-72121-7 (print) / 978-3-319-72122-4 (eBook). https://doi.org/10.1007/978-3-319-72122-4

## Overview

This repository works through the book **sequentially, subsection by subsection**. For each
subsection you get, per paragraph: a plain-English rewrite, an "explain it to a 10-year-old"
version, the list of key ideas, a full step-by-step derivation of every formula, a symbol-by-symbol
glossary, the "why does this equation exist" intuition, a visualization, a working Python
implementation, an equivalent R implementation, numerical verification, and practice problems
with worked solutions.

## Learning Objectives

By working through this repository you should be able to:

1. Translate a verbal description of disease transmission into a compartmental (ODE) model.
2. Derive, from first principles, the equations of the five classic epidemic models (Ch. 2).
3. Analyze equilibria, stability, bifurcations, and thresholds ($\mathcal{R}_0$) using the
   mathematical tools of Chapter 3 (linearization, Lyapunov functions, Floquet theory,
   phase-plane analysis, monotone systems).
4. Fit epidemic models to real/simulated data using linear and nonlinear least squares (Ch. 4).
5. Extend the basic toolkit to higher-dimensional and in-host models (Ch. 5).
6. Implement every model and every analysis technique independently in **both Python and R**.

## Repository Structure

```
Mathematical_Modeling_Study/
│
├── README.md                  <- you are here
├── LICENSE
├── requirements.txt            <- Python dependencies (pip)
├── environment.yml             <- Python dependencies (conda)
├── DESCRIPTION                 <- R package-style dependency manifest
├── renv/                       <- R environment lockfile setup (renv::init() target)
│
├── data/                       <- any datasets used for parameter estimation (Ch. 4)
├── figures/                    <- shared/cross-chapter figures
├── docs/                       <- rendered notes / progress tracker
├── references/                 <- citation info, further reading
│
├── python/                      <- ALL Jupyter notebooks, organized by chapter
│   ├── chapter_01/section_1_1.ipynb, section_1_2.ipynb, ...
│   ├── chapter_02/...
│   ├── chapter_03/...
│   ├── chapter_04/...
│   └── chapter_05/...
│
├── r/                           <- ALL R Markdown files, organized by chapter (mirrors python/)
│   ├── chapter_01/section_1_1.Rmd, section_1_2.Rmd, ...
│   └── ...
│
├── chapter_01/  Important Concepts in Mathematical Modeling of Infectious Diseases
├── chapter_02/  Five Classic Epidemic Models and Their Analysis
├── chapter_03/  Basic Mathematical Tools and Techniques
├── chapter_04/  Parameter Estimation and Nonlinear Least-Squares Methods
├── chapter_05/  Special Topics (higher-dim SEIR, in-host models, backward bifurcation)
│
└── each chapter_NN/ (non-code assets only) contains:
    ├── images/     diagrams/figures for that chapter's notebooks
    ├── exercises/  practice problems (conceptual, computational, coding, visualization, challenge)
    └── solutions/  full worked solutions
```

Notebooks live under `python/chapter_NN/` and `r/chapter_NN/`; each notebook references figures,
exercises, and solutions in the matching `chapter_NN/` folder via relative paths
(e.g. `../../chapter_01/exercises/section_1_1_exercises.md` from `python/chapter_01/`).

## Installation

### Python Setup

```bash
# Option A: pip
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Option B: conda
conda env create -f environment.yml
conda activate mathmodeling
```

### R Setup

```r
# From the repository root, in R:
install.packages("renv")
renv::init()      # will read DESCRIPTION and install pinned packages
```

Packages used: `ggplot2`, `dplyr`, `tidyr`, `deSolve`, `pracma`, `plotly`, `latex2exp`, `rmarkdown`.

### Running the Notebooks

```bash
jupyter lab python/chapter_01/section_1_1.ipynb
```

R Markdown files knit from RStudio (Knit button) or from the command line:

```r
rmarkdown::render("r/chapter_01/section_1_1.Rmd")
```

## Progress Tracker

**Status: COMPLETE.** All 5 chapters, 19 sections, from 1.1 through 5.2, are done — the entire
textbook has been rewritten, derived from first principles, explained intuitively, visualized,
implemented in both Python and R, numerically verified, and equipped with practice problems and
full worked solutions. See `docs/PROGRESS.md` for the full build log, including several real
numerical bugs (mislabeled parameter regimes, a stiff-ODE solver hang, a spurious optimizer local
minimum, a mistranscribed closed-form formula) that were caught, diagnosed, and fixed along the
way rather than papered over.

| Chapter | Section | Status |
|---|---|---|
| 1 | 1.1 Mathematical Modeling of Infectious Diseases: Issues and Approaches | ✅ done |
| 1 | 1.2 Deterministic Epidemic Models: Compartmental Approach | ✅ done |
| 1 | 1.3 An Example: Kermack–McKendrick Model | ✅ done |
| 1 | 1.4 Important Concepts in Compartmental Epidemic Models | ✅ done |
| 2 | 2.1 Kermack-McKendrick (detailed) | ✅ done |
| 2 | 2.2 SIS model | ✅ done |
| 2 | 2.3 model with demography | ✅ done |
| 2 | 2.4 varying population (homogeneous systems) | ✅ done |
| 2 | 2.5 Ross-MacDonald malaria model | ✅ done |
| 3 | 3.1-3.2 stability definitions + linearization | ✅ done |
| 3 | 3.3 Lyapunov functions + LaSalle | ✅ done |
| 3 | 3.4 Floquet theory | ✅ done |
| 3 | 3.5-3.6 phase-line/phase-plane, Poincare-Bendixson | ✅ done |
| 3 | 3.7-3.8 uniform persistence + Metzler/monotone systems | ✅ done |
| 4 | 4.1 linear least squares | ✅ done |
| 4 | 4.2 nonlinear least squares (Gauss-Newton) | ✅ done |
| 4 | 4.3 epidemic parameter estimation | ✅ done |
| 5 | 5.1 SEIR models | ✅ done |
| 5 | 5.2 in-host models, backward bifurcation | ✅ done |

## Dependencies

See `requirements.txt` / `environment.yml` (Python) and `DESCRIPTION` (R).

## References

- Li, M.Y. (2018). *An Introduction to Mathematical Modeling of Infectious Diseases*. Springer.
- See `references/` for the book's own bibliography, reproduced chapter by chapter as it is used.
