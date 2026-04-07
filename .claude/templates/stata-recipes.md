# Stata Recipes

Code patterns for data cleaning, regression, and export in Stata. Used by the Coder and Data-engineer agents.

---

## Data Cleaning Template

```stata
/*==============================================================================
  Project:  [Project Name]
  Purpose:  Clean raw data for analysis
  Input:    data/raw/[filename].dta
  Output:   data/clean/[filename]_clean.dta
  Author:   [Name]
  Date:     [Date]
==============================================================================*/

clear all
set more off
set maxvar 10000

* --- 1. Load and Inspect ---
use "data/raw/dataset.dta", clear
describe
codebook, compact
tab1 key_variables, missing

* --- 2. Variable Cleaning ---

// Rename for consistency
rename oldname newname

// Recode missing values (. = Stata missing)
mvdecode var1 var2 var3, mv(-99 -98 -97)

// Winsorize outliers at 1st/99th percentile
foreach var of varlist income wages {
    qui sum `var', detail
    replace `var' = r(p1)  if `var' < r(p1)  & !missing(`var')
    replace `var' = r(p99) if `var' > r(p99) & !missing(`var')
}

// String standardization
replace name = strtrim(stritrim(upper(name)))

// Generate categorical variables
gen age_group = .
replace age_group = 1 if inrange(age, 18, 29)
replace age_group = 2 if inrange(age, 30, 44)
replace age_group = 3 if inrange(age, 45, 64)
replace age_group = 4 if age >= 65 & !missing(age)
label define age_lbl 1 "18-29" 2 "30-44" 3 "45-64" 4 "65+"
label values age_group age_lbl

* --- 3. Missing Data ---
misstable summarize
misstable patterns

* --- 4. Derived Variables ---
gen log_income = ln(income)
gen treat_post = treated * post

* --- 5. Validation ---
isid id year           // Confirm unique panel identifier
assert !missing(id)
assert inrange(year, 2000, 2023)
assert income >= 0 if !missing(income)

* --- 6. Label and Save ---
label variable log_income "Log household income"
label variable treat_post "Treatment x Post"
label data "Cleaned panel data, [project], [date]"

compress
save "data/clean/dataset_clean.dta", replace
```

---

## Regression Workflow

```stata
* --- OLS with clustered SEs ---
regress y treat_post x1 x2, vce(cluster state)
eststo m1

* --- Two-way FE with reghdfe ---
* ssc install reghdfe
* ssc install ftools
reghdfe y treat_post x1 x2, absorb(id year) vce(cluster state)
eststo m2

* --- IV / 2SLS ---
ivregress 2sls y x1 x2 (endog = instrument), vce(cluster state)
eststo m3
estat firststage  // First-stage F-statistic
```

## Export to LaTeX

```stata
* ssc install estout

esttab m1 m2 m3 using "tables/table1_main.tex", replace ///
    se star(* 0.10 ** 0.05 *** 0.01) ///
    label booktabs ///
    nomtitles ///
    scalars("N Observations" "r2 R-squared") ///
    note("") ///
    fragment  // fragment = no table wrapper (added in LaTeX)
```

## Recommended Packages

| Package | Purpose | Install |
|---------|---------|---------|
| `reghdfe` | High-dimensional fixed effects | `ssc install reghdfe` |
| `ftools` | Fast collapse/merge (reghdfe dependency) | `ssc install ftools` |
| `estout` | Table export (esttab) | `ssc install estout` |
| `coefplot` | Coefficient plots | `ssc install coefplot` |
| `unique` | Check unique identifiers | `ssc install unique` |
| `mdesc` | Missing data summary | `ssc install mdesc` |
| `winsor2` | Winsorization | `ssc install winsor2` |
| `gtools` | Fast group operations | `ssc install gtools` |

## Common Pitfalls

- **Missing values:** Stata drops missing silently. Always check `misstable` before regression.
- **Factor variables:** Use `i.` prefix for categoricals: `reg y i.treat##i.post`.
- **Clustering:** `vce(cluster state)` requires enough clusters (>30 rule of thumb).
- **reghdfe singleton:** reghdfe drops singleton groups by default. Report how many observations are dropped.
