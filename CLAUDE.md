# Economics AI Research Template

## Environment Configuration

Machine-specific paths (R, LaTeX, Python, etc.) are not configured in this template. Put your local setup in your user-global Claude config at `~/.claude/CLAUDE.md` so it stays out of version control.

---

## Rules Layout

Two folders hold behavioral rules; they load differently:

- **`.claude/rules/`** — auto-loaded into every session (universal rules: figures, tables, R style, content invariants, governance loops). Keep this set small.
- **`.claude/rules-reference/`** — loaded on-demand by specific agents/skills:
  - `working-paper-format.md` → `writer`, `writer-critic`
  - `coding-standards-{python,julia}.md` → `coder`, `coder-critic` (when working in that language)
  - `journal-profiles.md` → `/review --peer`, `editor`, `domain-referee`, `methods-referee`
  - `domain-profile.md` → `domain-referee`, `methods-referee`, `theorist`, `theorist-critic`
  - `personal-style-guide.md` → `writer`
  - `meta-governance.md` → reference-only (template maintenance)
  - `logging.md` → `/checkpoint`

When adding a new rule, default to `.claude/rules-reference/` and have the consuming agent/skill read it explicitly. Move to `.claude/rules/` only if it applies to *every* session.

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
