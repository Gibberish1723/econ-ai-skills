# Stata Configuration Template (AEA Replication Standard)

> **Usage:** Copy the code block below into `code/config.do` in your replication package.
> Modify the package lists, paths, and seed for your project.
> This file is called at the top of your master do-file: `do "code/config.do"`.
> Based on the AEA Data Editor's [template-config.do](https://github.com/AEADataEditor/replication-template).

---

```stata
* config.do — Project Configuration
* Call this at the top of your master do-file:
*   do "code/config.do"
* Ensures reproducible packages, paths, and random seed.

* ============================================================
* Stata version control
* ============================================================
* Set the version to ensure forward compatibility.
version 18    /* [CHANGE to your Stata version] */

* ============================================================
* System information
* ============================================================
di "=== Configuration Start ==="
di "Date: $S_DATE $S_TIME"
di "Stata version: `c(stata_version)'"
di "OS: `c(os)'"
di "Machine: `c(hostname)'"
di "Username: `c(username)'"

* ============================================================
* Random seed
* ============================================================
* Declare once here; do NOT set seed again in individual scripts.
set seed 12345    /* [CHANGE to your project seed] */

* ============================================================
* Package installation
* ============================================================

* --- SSC packages (conditional install) ---
* Only installs if not already present.
local ssc_packages ///
    reghdfe ///
    ftools ///
    estout ///
    coefplot ///
    gtools
    /* [ADD your SSC packages here] */

foreach pkg of local ssc_packages {
    capture which `pkg'
    if _rc == 111 {
        di "Installing `pkg' from SSC..."
        ssc install `pkg', replace
    }
}

* --- Unconditional installs (always reinstall) ---
* Use for packages that need specific versions or frequent updates.
local ssc_unconditional ///
    /* [ADD packages that must always be reinstalled] */

foreach pkg of local ssc_unconditional {
    ssc install `pkg', replace
}

* --- Net install packages ---
* For packages not on SSC (GitHub, personal sites, etc.).
* net install regsave, from("https://raw.githubusercontent.com/reifjulian/regsave/master") replace
* [ADD your net install commands here]

* --- Rebuild Mata library index ---
* Required after installing packages that include Mata code.
mata: mata mlib index

* ============================================================
* Directory structure
* ============================================================

* Project root — assumes Stata is run from the package root.
global root "`c(pwd)'"

* Subdirectories — use these globals in all scripts.
global dir_raw     "${root}/data/raw"
global dir_clean   "${root}/data/clean"
global dir_tables  "${root}/output/tables"
global dir_figures "${root}/output/figures"
global dir_logs    "${root}/logs"

* Create directories if they don't exist.
capture mkdir "${root}/data"
capture mkdir "${root}/data/raw"
capture mkdir "${root}/data/clean"
capture mkdir "${root}/output"
capture mkdir "${root}/output/tables"
capture mkdir "${root}/output/figures"
capture mkdir "${root}/logs"

* ============================================================
* Ado-path management (optional)
* ============================================================
* Redirect to a local ado/ directory within the package.
* This ensures replicators use the exact same user-written ado files.

* Uncomment to use local ado directory:
* sysdir set PERSONAL "${root}/ado/personal"
* sysdir set PLUS     "${root}/ado/plus"

* If you have custom ado files in the package:
* adopath ++ "${root}/ado"

* ============================================================
* Logging
* ============================================================

* Create a timestamped log file.
local logdate = string(date("`c(current_date)'", "DMY"), "%tdCCYY-NN-DD")
local logtime = subinstr("`c(current_time)'", ":", "", .)
log using "${dir_logs}/log_`logdate'_`logtime'.txt", text replace

* ============================================================
* Configuration complete
* ============================================================
di "Root directory: ${root}"
di "Seed: 12345"
di "=== Configuration Complete ==="
```

---

## Notes

### Calling from the Master Do-File

The master do-file (`code/00_master.do`) should begin with:

```stata
* 00_master.do — Master Script
* Reproduces all tables and figures.
* Expected runtime: [FILL IN] on [FILL IN machine description].

clear all
set more off
set maxvar 32767    /* adjust if needed */

* Load configuration
do "code/config.do"

* Run all scripts in order
timer on 1
do "code/01_clean.do"
do "code/02_merge.do"
do "code/03_analysis.do"
do "code/04_figures.do"
do "code/05_robustness.do"
timer off 1
timer list

log close _all
```

### Calling from Individual Scripts

Each analysis script should begin with:

```stata
* 01_clean.do — Data Cleaning
* Purpose: [FILL IN]
* Inputs:  ${dir_raw}/[FILL IN].dta
* Outputs: ${dir_clean}/[FILL IN].dta

* Configuration is loaded by master; if running standalone:
capture confirm global root
if _rc != 0 {
    do "code/config.do"
}
```

### Version Pinning

The `version` command at the top of `config.do` ensures that Stata interprets all code as if running on that version, even on newer installations. This is the primary mechanism for Stata reproducibility.

### Package Versions

Unlike R's `renv`, Stata has no built-in package version manager. Document exact versions in the README:

| Package | Version | Source |
|---------|---------|--------|
| `reghdfe` | 6.12.3 | SSC |
| `ftools` | 2.49.1 | SSC |

For critical reproducibility, include the `.ado` files directly in an `ado/` directory within the package.
