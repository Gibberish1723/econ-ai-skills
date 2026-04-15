# Economics AI Research Template

## Environment Configuration

### R
- R is installed at: `<path-to-R-bin>`
- Always use `<path-to-Rscript>` for all R script execution
- When constructing Bash commands for R, use this explicit path

### LaTeX
- **Do NOT compile LaTeX via VS Code's built-in compiler** — use command-line only
- Standard compilation: `xelatex`, `pdflatex`, or `latexmk` via Bash
- Use `/tools compile [file]` for the standard 3-pass compilation workflow
- BibTeX: `bibtex` via Bash

### Platform
- <platform / shell>
- Use forward slashes in paths within bash commands

---

## Crash Resilience & Masterplan

- **Sessions can crash at any time.** Maintain a living masterplan in `quality_reports/masterplan.md` for the project
- Save all session plans to `quality_reports/plans/` and cross-check against the masterplan at each checkpoint
- **All R scripts must be checkpoint-based:** each file runs independently of the others
- Each script loads its own libraries and reads its own data from disk (or from a checkpoint `.rds`/`.csv` file) — never depend on another script's in-memory state
- Use checkpoint files between pipeline stages: script A writes `data/clean/step1_output.rds`, script B reads it

---

## R Code Style

- **Section headers:** use `# Section Name ----` (four hyphens after the comment). This enables RStudio code folding
- **Never** use `# ===== Section Name =====` or similar decorative headers with `=` above/below section names
- Correct: `# Load Data ----`
- Wrong: `# ===== Load Data =====`

---

## Figure Defaults

- **Always include axis lines** — `theme(axis.line = element_line(color = "black"))`
- **Never include gridlines** — `panel.grid.major = element_blank(), panel.grid.minor = element_blank()`
- **No subtitles** unless explicitly requested — use `labs(subtitle = NULL)`
- **Always save as PDF** — `ggsave(..., device = "pdf")`. Never PNG or JPG for publication figures
- **Use `theme_minimal()`** as the base theme — do not use serif fonts in figures

---

## Script Numbering & Run-All

- **Every script in `scripts/` must be numbered** with a two-digit prefix: `01_clean.R`, `02_merge.R`, `03_analysis.R`
- **Sub-steps** use a letter suffix: `03a_main_regs.R`, `03b_robustness.R`, `03c_heterogeneity.R`
- Numbers define execution order — `01` runs before `02`, `03a` before `03b`
- **A `00_run_all.R` master script must exist** in `scripts/` that sources every numbered script in order. Keep it up to date whenever scripts are added, removed, or renumbered
- **Periodic consistency checks:** whenever creating or modifying scripts, verify:
  - No gaps in numbering (e.g., `01`, `02`, `04` with no `03` — renumber or document why)
  - No duplicate numbers
  - `00_run_all.R` includes every numbered script and the order matches the file prefixes
  - Every script's inputs are produced by an earlier-numbered script (or come from `data/raw/`)
- **When adding a new script**, pick the correct number for its place in the pipeline, renumber downstream scripts if necessary, and update `00_run_all.R`

---

## Scope & Execution Discipline

- **Only modify the specific files mentioned** — do NOT apply changes to other files unless explicitly asked. Confirm scope before batch-applying style changes
- **When asked to create a script, write the file only** — do not run it or load data unless explicitly asked to run it
- **When asked to "consider results" or "analyze results"**, focus on substantive research findings (coefficients, figures, tables) — not error logs or warnings
- **Do not compile LaTeX** or run full pipelines unless explicitly asked
- **Do NOT commit or push to git** unless explicitly asked

---

## Estimation Philosophy

- **Design before results.** The identification strategy must be intentional before any results are examined.
- **Do NOT express concern or excitement about point estimates.** Focus on whether the specification is correct.
- **Results are meaningless until the design is intentional.** A surprising coefficient is not a finding — a well-identified coefficient is.
- **Mundane before substantive.** Rule out data artifacts and mechanical explanations before claiming a causal effect.

---

## Quick Start

| Command | What It Does |
|---------|-------------|
| `/discover [interview\|lit\|data]` | Research spec, literature review, or data discovery |
| `/strategize [question]` | Identification strategy + review |
| `/analyze [dataset]` | End-to-end analysis: scripts, output, code review |
| `/fletcher [output]` | Defamiliarization audit — interrogate results before writing |
| `/write [section]` | Draft paper sections + humanizer pass |
| `/review [file]` | Multi-agent quality review + weighted score |
| `/review --referee2` | Adversarial five-audit protocol (code, replication, package, automation, econometrics) |
| `/revise [report]` | Route referee comments, draft response letter |
| `/talk [format]` | Beamer presentation from paper (4 formats) |
| `/submit [journal]` | Final gate: score >= 95, all components >= 80 |
| `/tools [subcommand]` | commit, compile, validate-bib, journal, learn, deploy, context |

Enter at any stage. Use `/new-project` for the full pipeline.

---

## Project Context

Fill in per project:

### Research Question
[What is the causal question? What is the estimand?]

### Data Sources
[Dataset name, access level, sample period, unit of observation]

### Identification Strategy
[DiD, IV, RDD, Synthetic Control, Event Study — with key assumptions]

### Key Decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| | | |

### Dropped Analyses
[List analyses considered but dropped, so Claude doesn't re-suggest them]

### Variable Definitions

| Variable | Definition | Source |
|----------|-----------|--------|
| | | |

### Sample Restrictions
[Who is included/excluded and why]

### Key Files

| File | Purpose |
|------|---------|
| `scripts/` | Analysis scripts |
| `data/raw/` | Raw data |
| `data/clean/` | Cleaned data |
| `tables/` | Generated LaTeX tables |
| `figures/` | Generated figures |
| `Paper/` | LaTeX manuscript |
| `Talks/` | Beamer presentations |

### Current Status
[Discovery / Strategy / Execution / Peer Review / Submission]

---

## Notes for Claude
[Any additional project-specific instructions]
