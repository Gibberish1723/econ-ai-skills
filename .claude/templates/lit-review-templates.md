# Literature Review Templates

Structured templates for organizing literature searches and synthesis. Used by the Librarian agent.

---

## Paper Summary Template

For each paper reviewed, capture:

```markdown
### [Author(s)] ([Year]) — [Short Title]
**Journal:** [Journal Name]
**Question:** [One sentence]
**Data:** [Dataset, sample, period]
**Method:** [Identification strategy]
**Key Finding:** [Main result with magnitude]
**Relevance:** [How it connects to your paper — supports, contradicts, extends]
**Limitation:** [Main weakness or gap your paper addresses]
```

### Example

```markdown
### Card & Krueger (1994) — Minimum Wages and Employment
**Journal:** AER
**Question:** Do minimum wage increases reduce fast-food employment?
**Data:** NJ and PA fast-food restaurants, phone survey, 1992
**Method:** Natural experiment (DiD), NJ minimum wage increase vs PA control
**Key Finding:** No significant negative employment effect; point estimate slightly positive
**Relevance:** Seminal challenge to competitive labor market model; motivates our re-examination with admin data
**Limitation:** Phone survey data, single industry, short-run effects only
```

---

## Synthesis Matrix

Organize papers by dimension to identify patterns and gaps:

| Paper | Method | Data | Finding | Supports Your Hypothesis? |
|-------|--------|------|---------|--------------------------|
| Card & Krueger (1994) | DiD | Survey | No employment effect | Yes |
| Neumark & Wascher (2000) | DiD (payroll) | BLS | Negative employment | No |
| Cengiz et al. (2019) | Bunching | CPS | Minimal disemployment | Yes |
| Dube et al. (2010) | Border DiD | QCEW | No significant effect | Yes |

**Consensus:** [Summary of where literature stands]
**Gap:** [What's missing that your paper fills]

---

## Search Strategy Checklist

1. **Top-5 generals:** AER, Econometrica, JPE, QJE, REStud
2. **Top field journals:** [Per your domain-profile.md]
3. **Working papers:** NBER, IZA, CEPR, SSRN, RePEc
4. **Citation chains:** Forward (Google Scholar "cited by") and backward (reference lists)
5. **Connected Papers:** Visual similarity graph from seed paper

### Google Scholar Operators

| Operator | Example | Purpose |
|----------|---------|---------|
| `author:` | `author:"Daron Acemoglu"` | Find papers by specific author |
| `intitle:` | `intitle:"minimum wage" employment` | Title must contain phrase |
| Exact phrase | `"difference-in-differences"` | Exact match |
| Year range | `2018..2024` | Restrict to recent papers |
| Exclude | `-survey` | Remove survey papers |
| Site | `site:nber.org` | Restrict to NBER |

### Citation Network Building

```
Seed paper → backward references → forward citations → 
identify clusters → read most-cited in each cluster → 
identify frontier → position your contribution
```

---

## Research Gaps Framework

| Gap Type | Question to Ask | Example |
|----------|----------------|---------|
| **Data** | Is there better/newer data? | Admin data vs survey data |
| **Method** | Can identification be improved? | Modern DiD vs naive TWFE |
| **Context** | Does the finding hold elsewhere? | US finding → EU setting |
| **Mechanism** | Is the channel understood? | Effect exists but why? |
| **Heterogeneity** | Who is most affected? | Effects by race/gender/income |
| **Time horizon** | Short-run vs long-run? | Immediate vs 5-year effects |
