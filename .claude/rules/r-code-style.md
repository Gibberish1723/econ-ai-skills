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

## AEA Replication Patterns

These patterns ensure scripts meet AEA Data Editor standards for replication packages. See `templates/template-config-R.md` for the full config file template.

### Config File

- Every script begins with `source("code/config.R")` (or project-appropriate path)
- The config file declares all packages, paths, seed, and version requirements
- No package loading outside config (exception: script-specific packages documented in config)

### Version Documentation

- End of each script (or at minimum end of master script): call `log_session_info()` to capture `sessionInfo()` to a log file
- For submission-quality reproducibility: use `renv` with `renv.lock` committed to the package
- Alternative: use Posit Package Manager date-based snapshots for pinned versions

### Master Script

- Single `00_master.R` that sources all scripts in order
- Include estimated runtime as a comment at the top: `# Expected runtime: ~45 min on [machine]`
- Include runtime logging with `Sys.time()` bookends:

```r
# 00_master.R — Master Script ----
# Reproduces all tables and figures.
# Expected runtime: ~45 min on Intel i7 / 16 GB RAM.

source("code/config.R")
t0 <- Sys.time()

source("code/01_clean.R")
source("code/02_analysis.R")
source("code/03_figures.R")

message("Total runtime: ", round(difftime(Sys.time(), t0, units = "mins"), 1), " minutes")
log_session_info()
```

### Output Traceability

- Every `ggsave()` and table-writing call should have a comment noting which table/figure number in the paper it produces
- Example: `ggsave(file.path(dir_figures, "fig2_event_study.pdf"), ...) # Figure 2`
- Example: `writeLines(tab_tex, file.path(dir_tables, "table1_summary.tex")) # Table 1`
- This enables the verifier to cross-reference outputs against the paper automatically
