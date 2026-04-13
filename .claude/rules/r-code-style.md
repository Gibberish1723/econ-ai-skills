# R Code Style

## Section Headers

- Use `# Section Name ----` for code sections (RStudio-compatible folding)
- The four hyphens `----` go **after** the section name comment, on the same line
- **Never** use `# ===== Section Name =====` or decorative headers with `=` above/below

**Correct:**
```r
# Load Libraries ----
library(tidyverse)
library(fixest)

# Import Data ----
df <- readRDS("data/clean/main_sample.rds")

# Estimation ----
mod <- feols(y ~ treatment | fe, data = df)
```

**Wrong:**
```r
# ===== Load Libraries =====
# =====================
# Load Libraries
# =====================
```

## Self-Contained Scripts (Checkpoint Pattern)

- **Each R script must run independently** — never depend on another script's in-memory state
- Every script loads its own libraries at the top
- Every script reads its own data from disk (raw files or checkpoint files from a prior stage)
- Use `.rds` or `.csv` checkpoint files between pipeline stages

**Example pipeline:**
```
scripts/01_clean.R      → writes data/clean/step01_cleaned.rds
scripts/02_merge.R      → reads step01, writes data/clean/step02_merged.rds
scripts/03_analysis.R   → reads step02, writes tables/reg_main.tex
scripts/04_figures.R    → reads step02, writes figures/fig1_event_study.pdf
```

Each script starts with:
```r
# Load Libraries ----
library(tidyverse)

# Read Data ----
df <- readRDS("data/clean/step01_cleaned.rds")
```

## General Style

- `set.seed()` once at the top of any script with stochastic elements
- All packages loaded at the top, never mid-script
- No hardcoded absolute paths — use relative paths from the project root
