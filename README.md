# Economics AI Research Template

An AI-assisted research workflow for empirical economics, built on [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Manages the full lifecycle from research ideation through journal submission using 16 specialized agents with adversarial quality control.

Forked from [Pedro Sant'Anna's claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow).

---

## Quick Start

1. **Fork this repo** and clone it locally
2. **Install [Claude Code](https://docs.anthropic.com/en/docs/claude-code)** (CLI, VS Code extension, or desktop app)
3. **Fill in `CLAUDE.md`** with your project details (research question, data sources, identification strategy)
4. **Fill in `.claude/rules/domain-profile.md`** with your field's journals, datasets, and conventions
5. Start working — enter at any stage:

| Command | What It Does |
|---------|-------------|
| `/discover [interview\|lit\|data]` | Research spec, literature review, or data discovery |
| `/strategize [question]` | Identification strategy design + review |
| `/analyze [dataset]` | End-to-end analysis: scripts, output, code review |
| `/fletcher [output]` | Defamiliarization audit — interrogate results before writing |
| `/write [section]` | Draft paper sections + humanizer pass |
| `/review [file]` | Multi-agent quality review + weighted score |
| `/revise [report]` | Route referee comments, draft response letter |
| `/talk [format]` | Beamer presentation (job market, seminar, short, lightning) |
| `/submit [journal]` | Final gate: score >= 95, all components >= 80 |
| `/tools [subcommand]` | commit, compile, validate-bib, journal, context |

Use `/new-project` to run the full pipeline from scratch.

---

## Architecture

### 16 Specialized Agents

Seven worker-critic pairs enforce separation of powers: creators produce artifacts, critics score them. Neither crosses the line.

| Worker | Critic | Domain |
|--------|--------|--------|
| Librarian | Librarian-Critic | Literature search and coverage |
| Explorer | Explorer-Critic | Data discovery and feasibility |
| Data-Engineer | Coder-Critic | Data cleaning and wrangling |
| Strategist | Strategist-Critic | Identification strategy |
| Coder | Coder-Critic | Analysis scripts (R, Stata, Python) |
| Writer | Writer-Critic | Paper manuscript |
| Storyteller | Storyteller-Critic | Beamer presentations |

Plus:
- **Orchestrator** — dependency graph, agent dispatch, phase routing
- **Domain Referee** + **Methods Referee** — independent blind peer review
- **Verifier** — replication package validation and submission gate

### Quality Gates

| Gate | Threshold |
|------|-----------|
| Commit | >= 80 |
| Pull Request | >= 90 |
| Journal Submission | >= 95 (every component >= 80) |

Scores aggregate across components with weights favoring identification validity (25%) and paper quality (25%). Three-strikes escalation when a worker-critic pair can't converge.

### Journal-Calibrated Review

`/review --peer [journal]` makes referees emulate a specific journal's review culture. 15 pre-populated profiles (AER, QJE, JPE, Econometrica, REStud, AEJ:Applied, AEJ:Policy, JHR, JHE, RAND, JPubE, JLE, JDE, RESTAT, AER:Insights). Add your own in `.claude/rules/journal-profiles.md`.

---

## Customization

This template is economics-focused but designed to generalize. To adapt it for your field:

- **`.claude/rules/domain-profile.md`** — your field's journals, common datasets, identification strategies, notation, and seminal references
- **`.claude/rules/journal-profiles.md`** — add profiles for journals in your field using the template at the bottom
- **`CLAUDE.md`** — project-specific context (research question, data, sample restrictions, key decisions)
- **`.claude/agents/`** — modify agent prompts to match your methodology
- **`.claude/skills/`** — add or modify slash commands

---

## Project Structure

```
CLAUDE.md                  # Project context (fill in per project)
scripts/                   # Analysis scripts
data/raw/                  # Raw data
data/clean/                # Cleaned data
tables/                    # Generated LaTeX tables
figures/                   # Generated figures
Paper/                     # LaTeX manuscript
Talks/                     # Beamer presentations
quality_reports/           # Session logs, plans, research journal
.claude/
  agents/                  # 16 agent definitions
  skills/                  # 10 slash commands
  rules/                   # Behavioral rules and standards
  guide/                   # Quarto documentation site
```

---

## Documentation

The `.claude/guide/` directory contains a Quarto site with detailed documentation:

- **User Guide** — workflow patterns and getting started
- **Architecture** — system design and phase dependencies
- **Meet the Agents** — all 16 agents with roles and responsibilities
- **Customization** — adapting the template for your field
- **Command Reference** — every command, flag, and subcommand

Deploy with `/tools deploy` to GitHub Pages.

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI, VS Code, JetBrains, or desktop app)
- R, Stata, or Python (for analysis scripts)
- LaTeX distribution (for paper and talk compilation)

---

## License

MIT

---

## Acknowledgments

- [Pedro Sant'Anna](https://github.com/pedrohcgs) — original [claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow) template
- [Antonio Mele](https://github.com/meleantonio) — contributions and collaboration
