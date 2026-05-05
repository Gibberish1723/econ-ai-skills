---
name: submit
description: Submission pipeline — journal targeting, replication package, audit, and final gate. Replaces /submit, /target-journal, /audit-replication, /data-deposit.
disable-model-invocation: true
argument-hint: "[mode: target | package | audit | final] [journal name (optional)]"
---

# Submit

Submission pipeline with four modes covering journal selection through final verification.

**Input:** `$ARGUMENTS` — mode keyword, optionally followed by journal name.

---

## Modes

### `/submit target` — Journal Targeting
Get ranked journal recommendations.

**Agent:** Orchestrator (journal selection function)

Considers: contribution fit, methodology fit, audience fit, recent publications, desk rejection risk. Consults .claude/rules-reference/domain-profile.md for journal tiers.

Output: Ranked list of 3 target journals with rationale.
Save to `quality_reports/journal_recommendations_[date].md`

### `/submit package` — Build Replication Package
Assemble AEA-compliant replication package following the [AEA Data Editor replication template](https://github.com/AEADataEditor/replication-template).

**Agents:** Coder + Verifier

**AEA directory structure:**
```
Replication/
  README.md              ← from templates/aea-readme.md
  LICENSE.txt            ← code license (Modified BSD default)
  code/
    config.R             ← from templates/template-config-R.md
    00_master.R          ← master script, runs all below
    01_clean.R
    02_analysis.R
    03_figures.R
    ...
  data/
    raw/                 ← included public data only
    README_data.md       ← data-specific notes (optional)
  output/
    tables/
    figures/
  logs/                  ← sessionInfo logs from runs
```

**Steps:**
1. Copy and organize scripts into `Replication/code/` with config file
2. Create master script (`00_master.R`) that sources all scripts in order
3. Generate README from `templates/aea-readme.md` — fill in computational requirements, program descriptions, dataset list
4. Generate **cross-reference table** — scan scripts for output writes, match against paper table/figure references
5. Generate **data citation list** — extract dataset references, format per [AEA Sample References](https://www.aeaweb.org/journals/data/references)
6. Copy included public data to `Replication/data/raw/`
7. Add `LICENSE.txt` (Modified BSD default)
8. Run Verifier in submission mode (12 checks) on the assembled package
Save to `Replication/`

### `/submit audit` — Audit Replication Package
Verify replication package completeness against AEA Data Editor standards.

**Agent:** Verifier (submission mode — 12 checks)

Checks:
1. LaTeX compilation (paper compiles cleanly)
2. Script execution (all scripts run without error)
3. File integrity (all references resolve)
4. Output freshness (outputs match latest code)
5. Package inventory (master script, config file, no orphans)
6. Dependency verification (renv.lock / sessionInfo / requirements.txt)
7. Data provenance (citations with DOI, access conditions, restricted data docs)
8. Execution verification (master script end-to-end, runtime reported)
9. Output cross-reference (every paper table/figure traced to script)
10. README completeness (all AEA-required sections present)
11. PII scan (variable names and data patterns — WARN level)
12. openICPSR metadata readiness (JEL codes, title format, subject terms)

### `/submit final [journal]` — Final Submission Gate
Full verification + score enforcement + submission checklist.

Workflow:
1. Run comprehensive review if not done recently
2. Run replication audit (12 checks)
3. Verify README matches AEA template structure (all required sections present)
4. Check score gate: aggregate >= 95, all components >= 80
5. If PASS: generate cover letter draft + submission checklist + openICPSR deposit instructions
6. If FAIL: list blocking issues and stop

---

## Principles
- **Score >= 95 + all components >= 80. No exceptions.**
- **Don't skip verification.** Even if reports exist, check they're recent.
- **If it fails, stop.** Don't generate materials for a failing paper.
- **Cover letter is a draft.** User must review before sending.
