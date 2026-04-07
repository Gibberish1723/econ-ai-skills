---
name: review
description: All quality reviews — routes to appropriate critics based on target file type and flags. Replaces /paper-excellence, /proofread, /econometrics-check, /review-r, /review-paper.
disable-model-invocation: true
argument-hint: "[file path or --flag] Options: --peer [journal], --methods, --proofread, --code, --replicate [lang], --all"
---

# Review

Unified review command that routes to the appropriate critic agents based on the target and flags.

**Input:** `$ARGUMENTS` — file path and/or flags.

---

## Routing Logic

### Auto-detect by file type
- `.tex` paper file → **Comprehensive review** (writer-critic + strategist-critic + Verifier)
- `.R`, `.py`, `.do`, `.jl` file → **Code review** (coder-critic standalone, categories 4-12)
- `.tex` talk file (in Talks/) → **Talk review** (storyteller-critic)

### Explicit flags (override auto-detect)
- `--peer` → **Full peer review** (domain-referee + methods-referee, independent blind reports + editorial synthesis)
- `--peer [journal]` → **Journal-calibrated peer review** (same, but referees emulate that journal's review culture via journal-profiles.md)
- `--methods` → **Causal audit** (strategist-critic standalone, 4-phase review)
- `--proofread` → **Manuscript polish** (writer-critic standalone, 6 categories)
- `--code [file]` → **Code review** (coder-critic standalone, categories 4-12)
- `--replicate [language]` → **Cross-language replication** (Coder re-implements in target language + coder-critic + comparison)
- `--referee2` → **Referee 2 audit** (five-audit adversarial protocol — code, cross-language, directory, output automation, econometrics)
- `--all` or no file → **Paper excellence** (all critics in parallel + weighted score)

---

## Mode Details

### Comprehensive Review (default for .tex paper)
Dispatch in parallel:
1. **strategist-critic** — causal design audit (4 phases)
2. **writer-critic** — manuscript polish (6 categories)
3. **Verifier** — compilation check
Compute weighted aggregate score.

### Full Peer Review (`--peer` or `--peer [journal]`)
Simulates journal peer review:
1. Dispatch **domain-referee** — subject expertise review (5 dimensions, weighted)
2. Dispatch **methods-referee** — econometric methods review (5 dimensions, weighted)
3. Both reviews are independent and blind
4. If a journal name is provided, pass it to both referees — they read `journal-profiles.md` and calibrate to that journal's review culture
5. Orchestrator synthesizes editorial decision: Accept / Minor / Major / Reject
6. Save reports to `quality_reports/`

### Code Review (`--code` or auto-detect .R/.py/.do)
Dispatch **coder-critic** in standalone mode:
- Categories 4-12 only (code quality, no strategy comparison)
- Save report to `quality_reports/[file]_code_review.md`

### Causal Audit (`--methods`)
Dispatch **strategist-critic** standalone:
- Full 4-phase review (claim, design, inference, polish)
- Save report to `quality_reports/[file]_strategy_review.md`

### Manuscript Polish (`--proofread`)
Dispatch **writer-critic** standalone:
- 6 categories: structure, claims-evidence, ID fidelity, writing, grammar, compilation
- Save report to `quality_reports/[file]_proofread_report.md`

### Cross-Language Replication (`--replicate [language]`)
Re-implement existing code in a different language and compare outputs:
1. Auto-detect source language from file extension (`.R`, `.py`, `.do`, `.jl`)
2. Dispatch **Coder** in replication mode — re-implement in target language
3. **coder-critic** reviews both implementations
4. Compare numerical outputs per `domain-profile.md` Quality Tolerance Thresholds
5. Save replicated script to `scripts/[target-language]/`
6. Save report to `quality_reports/[file]_replication_report.md`

Divergences are flagged with exact values. The report includes a side-by-side table.

### Referee 2 Audit (`--referee2`)
Adversarial five-audit protocol inspired by Scott Cunningham's MixtapeTools. Referee 2 is a "health inspector for empirical research" — blunt, systematic, and never modifies author code.

**Five Audits:**

1. **Code Audit** — Missing value handling, merge diagnostics, variable construction, filter conditions. Read and run author's code without modification.
2. **Cross-Language Replication** — Re-implement key specifications in a second language (R↔Stata↔Python). Compare outputs to 6+ decimal places. Exploits "orthogonal hallucination errors" across language implementations.
3. **Directory & Replication Package Audit** — Folder structure, relative paths (no absolute), naming conventions, master script that runs everything, README completeness.
4. **Output Automation Audit** — Verify all tables and figures are programmatically generated from scripts. No manual numbers, no copy-paste from console.
5. **Econometrics Audit** — Identification strategy validity, standard error clustering, fixed effects specification, parallel trends, balance checks, first-stage diagnostics.

**Key Principles:**
- Referee 2 NEVER modifies author code — only reads, runs, and creates independent replication scripts
- Personality: blunt, skeptical by default, proportional, systematic
- Produces a formal referee report with specific line-number citations
- Relationship to Fletcher: Fletcher runs DURING analysis (author's self-check); Referee 2 runs AFTER completion (adversarial audit in fresh context)

**Output:** Save report to `quality_reports/referee2_report.md`

---

## Scoring

| Mode | Blocking? | Gate |
|------|-----------|------|
| Comprehensive | Yes | 80 commit, 90 PR |
| Peer Review | Yes | Referee recommendation |
| Code Review | Yes | 80 commit |
| Causal Audit | Yes | 80 commit |
| Proofread | Yes (paper), Advisory (talks) | 80 commit |

---

## Principles
- **Smart routing.** File type determines the default review mode.
- **Flags override.** Use explicit flags for targeted reviews.
- **Critics never edit.** All reviews produce reports only.
- **Proportional severity.** Phase-aware deductions per quality.md.
