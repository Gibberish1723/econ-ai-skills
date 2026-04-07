# R Econometrics Recipes

Code patterns for common econometric analyses using `fixest`. Used by the Coder agent.

---

## Difference-in-Differences (Two-Way Fixed Effects)

```r
library(fixest)
library(modelsummary)

# Basic TWFE DiD
m1 <- feols(y ~ treat_post | unit + year, data = df, cluster = ~state)

# With controls
m2 <- feols(y ~ treat_post + x1 + x2 | unit + year, data = df, cluster = ~state)

# Multiple outcomes
m_multi <- feols(c(y1, y2, y3) ~ treat_post | unit + year, data = df, cluster = ~state)
```

## Event Study

```r
# Event study with relative time indicators
# Requires: event_time variable (periods relative to treatment)
m_es <- feols(y ~ i(event_time, ref = -1) | unit + year, 
              data = df, cluster = ~state)

# Plot event study coefficients
iplot(m_es, 
      xlab = "Periods Relative to Treatment",
      ylab = "Estimate and 95% CI",
      main = NULL)  # No title — goes in LaTeX caption
```

## Staggered DiD (Sun & Abraham)

```r
# WARNING: Naive TWFE is biased with staggered treatment timing
# Use Sun & Abraham (2021) interaction-weighted estimator

m_sa <- feols(y ~ sunab(first_treat, year) | unit + year, 
              data = df, cluster = ~state)

# Aggregate to event-time coefficients
summary(m_sa, agg = "att")
iplot(m_sa)
```

## Robustness: Two-Way Clustering

```r
# Cluster at two levels (e.g., state and year)
m_twoway <- feols(y ~ treat_post | unit + year, 
                  data = df, cluster = ~state + year)
```

## Export to LaTeX

```r
# Export regression table via modelsummary
models <- list(
  "Baseline" = m1,
  "With Controls" = m2,
  "Sun-Abraham" = m_sa
)

modelsummary(models,
  output = "tables/table1_main_results.tex",
  stars = c("*" = 0.1, "**" = 0.05, "***" = 0.01),
  coef_omit = "Intercept",
  gof_omit = "AIC|BIC|Log",
  title = NULL,  # Title goes in LaTeX \caption{}
  notes = NULL   # Notes go in LaTeX \tablenotes
)
```

## IV / 2SLS

```r
# Instrumental variables with fixest
m_iv <- feols(y ~ x1 | unit + year | endog ~ instrument, 
              data = df, cluster = ~state)

# Check first stage
summary(m_iv, stage = 1)
fitstat(m_iv, "ivf")  # First-stage F-statistic
```

## RDD (with rdrobust)

```r
library(rdrobust)

# Sharp RDD
rd <- rdrobust(y = df$y, x = df$running_var, c = 0)
summary(rd)

# RD plot
rdplot(y = df$y, x = df$running_var, c = 0,
       x.label = "Running Variable",
       y.label = "Outcome",
       title = NULL)  # No title — goes in LaTeX caption
```

## Common Pitfalls

- **Staggered TWFE:** Never use naive `feols(y ~ treat | unit + year)` with staggered adoption. Use `sunab()`, Callaway-Sant'Anna, or Borusyak et al.
- **Clustering:** Cluster at the level of treatment variation (usually state for state-level policies).
- **Pre-trends:** Always show event study with pre-treatment coefficients. Joint F-test for pre-trend = 0.
- **Multiple testing:** If testing 10+ outcomes, apply Bonferroni or BH correction.
