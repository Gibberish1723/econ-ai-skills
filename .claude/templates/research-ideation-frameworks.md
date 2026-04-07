# Research Ideation Frameworks

Structured approaches for generating economics research ideas. Used by the `/discover ideate` mode.

---

## Four Primary Approaches

### 1. Puzzle Approach
Start from a surprising fact or inconsistency in existing evidence.

**Template:**
- "The literature says X, but data shows Y. Why?"
- "Theory predicts A, but we observe B in [context]."

### 2. Policy Approach
Start from a policy change that creates identifying variation.

**Template:**
- "[Policy] changed in [year] in [place], creating a natural experiment for [question]."
- "States adopted [policy] at different times, allowing staggered DiD."

### 3. Data Approach
Start from a new or underexploited dataset.

**Template:**
- "[New dataset] has [unique feature] that allows measurement of [previously unmeasurable]."
- "Administrative data from [source] covers [population] with [granularity]."

### 4. Extension Approach
Extend existing work along a new dimension.

**Template:**
- "[Paper] showed X in [context A]. Does it hold in [context B]?"
- "[Paper] found short-run effects. What about long-run?"

---

## Advanced Frameworks

### "5 Whys" (Root Cause Analysis)

Starting from an empirical regularity, ask "why?" five times to drill toward mechanism:

1. Why do minimum wage increases not reduce employment?
2. Why can firms absorb higher labor costs?
3. Why don't firms pass through costs to consumers?
4. Why is labor demand more elastic in some markets?
5. Why does monopsony power vary across local labor markets?

Each "why" can become a research question.

### "What If" Generator

Reverse assumptions in a known paper:

| Original Assumption | What If... | New Question |
|-------------------|------------|--------------|
| Workers are mobile | Workers face moving costs | How do mobility frictions affect wage pass-through? |
| Firms are price-takers | Firms have market power | How does monopsony affect policy incidence? |
| Treatment is binary | Treatment varies in intensity | What is the dose-response relationship? |
| Effects are homogeneous | Effects vary by subgroup | Who benefits most from [policy]? |

### Cross-Field Pollinator

Take a method or finding from one field and apply it to another:

| Source Field | Insight | Target Application |
|-------------|---------|-------------------|
| IO (demand estimation) | BLP random coefficients | Health insurance plan choice |
| Finance (event studies) | High-frequency identification | Labor market policy announcements |
| Development (RCTs) | Pre-analysis plans | Observational studies with admin data |
| CS (machine learning) | Causal forests | Heterogeneous treatment effects |

---

## Evaluation Matrix

Score each idea before investing time:

| Criterion | Weight | Score (1-5) |
|-----------|--------|-------------|
| **Feasibility** (data exists, methods tractable) | 25% | |
| **Identification** (clean causal story possible) | 30% | |
| **Policy relevance** (someone cares about the answer) | 20% | |
| **Novelty** (gap in literature is real) | 15% | |
| **Scalability** (can become a full paper, not just a note) | 10% | |

**Threshold:** Pursue ideas scoring >= 3.5 weighted average.
