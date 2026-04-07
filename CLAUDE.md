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
