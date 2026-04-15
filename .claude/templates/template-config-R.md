# R Configuration Template (AEA Replication Standard)

> **Usage:** Copy the code block below into `code/config.R` in your replication package.
> Modify the `global.libraries` vector, paths, and seed for your project.
> This file is sourced at the top of every analysis script: `source("code/config.R")`.
> Based on the AEA Data Editor's [template-config.R](https://github.com/AEADataEditor/replication-template).

---

```r
# config.R — Project Configuration ----
# Source this file at the top of every script:
#   source("code/config.R")
# This ensures reproducible package versions, paths, and random seed.

# Record start time ----
config_start_time <- Sys.time()

# System information ----
message("=== Configuration Start ===")
message("Date: ", Sys.Date())
message("R version: ", R.version.string)
message("Platform: ", R.version$platform)
message("User: ", Sys.info()["user"])
message("Machine: ", Sys.info()["nodename"])

# Random seed ----
# Declare once here; do NOT set seed again in individual scripts.
set.seed(12345)  # [CHANGE to your project seed]

# Required packages ----
# List ALL packages used across ALL scripts in the project.
global.libraries <- c(
  "tidyverse",
  "fixest",
  "modelsummary",
  "kableExtra",
  "here",
  "haven"
  # [ADD your packages here]
)

# Package installation ----
# Installs any missing packages. For exact version replication, use renv instead.
# To use renv: delete this section and run renv::restore() from the project root.
missing_packages <- global.libraries[!(global.libraries %in% installed.packages()[, "Package"])]
if (length(missing_packages) > 0) {
  message("Installing missing packages: ", paste(missing_packages, collapse = ", "))
  install.packages(missing_packages, dependencies = TRUE)
}

# Load packages ----
invisible(lapply(global.libraries, library, character.only = TRUE))

# Project root ----
# Detect project root portably. Requires a .here file or .Rproj file in root.
# Alternative: set manually if here() detection fails.
# root <- here::here()
# Or for replication packages where structure is fixed:
root <- getwd()  # assumes Rscript is run from package root

# Directory structure ----
# Create output directories if they don't exist.
dirs <- c(
  file.path(root, "data", "clean"),
  file.path(root, "output", "tables"),
  file.path(root, "output", "figures"),
  file.path(root, "logs")
)

for (d in dirs) {
  dir.create(d, recursive = TRUE, showWarnings = FALSE)
}

# Path globals ----
# Use these in all scripts instead of hardcoded paths.
dir_raw     <- file.path(root, "data", "raw")
dir_clean   <- file.path(root, "data", "clean")
dir_tables  <- file.path(root, "output", "tables")
dir_figures <- file.path(root, "output", "figures")
dir_logs    <- file.path(root, "logs")

# Session info logging ----
# Captures full session info to a log file for reproducibility documentation.
log_session_info <- function() {
  log_file <- file.path(dir_logs, paste0("sessionInfo_", format(Sys.time(), "%Y%m%d_%H%M%S"), ".txt"))
  writeLines(capture.output(sessionInfo()), log_file)
  message("Session info saved to: ", log_file)
}

# Configuration complete ----
message("Root directory: ", root)
message("Packages loaded: ", paste(global.libraries, collapse = ", "))
message("Seed: 12345")
message("Config time: ", round(difftime(Sys.time(), config_start_time, units = "secs"), 1), " seconds")
message("=== Configuration Complete ===")
```

---

## Notes

### Using renv for Exact Replication

For submission-quality reproducibility, prefer `renv` over manual installation:

1. During development: `renv::init()` then `renv::snapshot()` after each package change
2. Commit `renv.lock` to the replication package
3. In `config.R`, replace the installation section with:
   ```r
   if (file.exists("renv.lock")) {
     renv::restore(prompt = FALSE)
   }
   ```
4. Replicators run `renv::restore()` once, then all scripts use the exact same versions

### Posit Package Manager (Date-Based Snapshots)

For reproducible package versions without renv:

```r
# Pin CRAN repository to a specific date
options(repos = c(CRAN = paste0(
  "https://packagemanager.posit.co/cran/",
  format(Sys.Date() - 31, "%Y-%m-%d")
)))
```

This ensures `install.packages()` pulls the same versions regardless of when the code runs.

### Calling from Scripts

Every analysis script should begin with:

```r
# Load Libraries ----
source("code/config.R")

# Read Data ----
df <- readRDS(file.path(dir_clean, "analysis_sample.rds"))
```

And end with:

```r
# Log Session ----
log_session_info()
```

### Master Script Pattern

The master script (`code/00_master.R`) sources all scripts in order:

```r
# 00_master.R — Master Script ----
# Reproduces all tables and figures.
# Expected runtime: [FILL IN] on [FILL IN machine description].

source("code/config.R")

t0 <- Sys.time()

source("code/01_clean.R")
source("code/02_merge.R")
source("code/03_analysis.R")
source("code/04_figures.R")
source("code/05_robustness.R")

message("Total runtime: ", round(difftime(Sys.time(), t0, units = "mins"), 1), " minutes")
log_session_info()
```
