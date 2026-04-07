---
name: fletcher
description: Defamiliarization audit for empirical output. Six structured steps to interrogate results before interpretation. Named after Jason Fletcher. Use when reviewing figures, tables, or estimation output before writing.
disable-model-invocation: true
argument-hint: "[file or output to audit]"
---

# Fletcher (Defamiliarization Audit)

A structured protocol to restore perception of research output before interpretation. Named after Jason Fletcher. Based on Viktor Shklovsky's principle: "habit devours everything."

**When to use:** After estimation output exists but BEFORE writing results sections. Fletcher catches what familiarity hides.

**Input:** `$ARGUMENTS` — path to script output, table, or figure to audit.

---

## The Six Steps

### Step 1: List Everything

Enumerate all visible features of the output **before interpreting**. Do not explain — just describe.

For a regression table: number of columns, rows, coefficient signs, magnitudes, significance levels, N, R-squared, fixed effects included, SE type.

For a figure: axes, scale, number of series, trends, discontinuities, confidence intervals, outliers.

**Output:** Numbered list of observed features. Mark DONE when complete.

### Step 2: What Would Generate This?

For each feature from Step 1, generate at least TWO explanations:
- One **mundane** (data artifact, coding choice, mechanical relationship)
- One **substantive** (real economic behavior, causal effect)

Do not judge which is correct yet. Just list both.

**Output:** Feature → [mundane explanation, substantive explanation]. Mark DONE when all features covered.

### Step 3: Find the Hardest One

Identify the single feature that is **hardest to explain** with a mundane story. This is where the real economics lives — or where the biggest data problem hides.

Investigate it:
- Check the code that produces it
- Look at sample restrictions
- Verify variable construction

**Output:** The hardest feature + investigation findings. Mark DONE or FLAG if concerning.

### Step 4: Own the Sample Size

Verify that N makes sense:
- Starting N (raw data) → Final N (estimation sample)
- Account for every observation lost: merges, missing values, sample restrictions, singleton drops
- Does the final N match what you'd expect given the population?

If N changed between specifications, explain why.

**Output:** N trace from raw to final. Mark DONE or FLAG if unexplained drops.

### Step 5: Check the Pattern

Assess coherence across specifications, subgroups, or variants:
- Do coefficients move sensibly as controls are added?
- Are heterogeneous effects consistent with a unified story?
- Do point estimates across specifications bracket a plausible range?
- Does the event study show pre-trends?

Incoherence is not automatically a problem — but it must be explainable.

**Output:** Coherence assessment. Mark DONE or FLAG if patterns are suspicious.

### Step 6: Ownership Test

Can you account for every number in the output?
- Trace each coefficient back to the specification that produced it
- Match N to sample construction
- Verify standard errors match clustering level
- Confirm significance stars match p-values

If any number cannot be traced, it's not your result — it's a stranger's.

**Output:** Ownership confirmation. Mark DONE or FLAG if any numbers are unaccounted for.

---

## Fletcher Report

After all six steps, produce a summary:

```markdown
## Fletcher Report — [Target File]
**Date:** [YYYY-MM-DD]

### Step Results
1. List Everything: DONE
2. Alternative Explanations: DONE
3. Hardest Feature: [description] — DONE/FLAG
4. Sample Size Trace: DONE/FLAG
5. Pattern Coherence: DONE/FLAG
6. Ownership Test: DONE/FLAG

### Ruling: [CLEAR / CONDITIONAL / HOLD]

### Key Findings
- [Most important observation]
- [Second most important]

### Flags (if any)
- [Description of concern + recommended action]
```

### Rulings

| Ruling | Meaning | Action |
|--------|---------|--------|
| **CLEAR** | All steps DONE, no FLAGS | Proceed to writing |
| **CONDITIONAL** | Minor FLAGS that can be addressed | Fix flagged items, then proceed |
| **HOLD** | Major FLAGS — results not ready | Investigate before writing |

---

## Relationship to Other Reviews

- **Fletcher** runs DURING analysis (before writing) — catches problems early
- **coder-critic** reviews code quality and reproducibility
- **Referee 2** (`/review --referee2`) runs AFTER completion — fresh-eyes audit
- Fletcher is the author's self-check; Referee 2 is the adversarial audit

---

## Principles

- **Describe before explaining.** Observation precedes interpretation.
- **Mundane before substantive.** Rule out the boring explanation first.
- **Every number has an owner.** If you can't trace it, investigate.
- **Incoherence is information.** Don't smooth over puzzling patterns — interrogate them.
