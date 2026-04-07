# The Rhetoric of Decks

A design philosophy for academic presentations. Referenced by the Storyteller agent.
Adapted from Scott Cunningham's MixtapeTools.

---

## Part I: The Three Laws

### Law 1: Beauty Is Function
Aesthetic quality is not decoration — it signals competence and respect for the audience. A beautiful slide is one where every element earns its place.

### Law 2: Cognitive Load Is the Enemy
One idea per slide. If a slide requires more than 7 seconds to parse, it has failed. Cut text, enlarge figures, remove decoration.

### Law 3: The Slide Serves the Spoken Word
Slides are not the talk — they are visual accompaniment. The speaker carries the narrative; slides carry the evidence. Never read your slides aloud.

---

## Part II: Aristotelian Foundation

Every academic talk deploys ethos, pathos, and logos in different proportions:

| Context | Ethos | Pathos | Logos |
|---------|-------|--------|-------|
| Job market talk | High (you ARE the product) | Medium (motivation) | High (rigor) |
| Seminar | Medium | Low | Very High |
| Conference (15 min) | Low (name on slide) | High (hook matters) | Medium (key result only) |
| Teaching lecture | Medium | High (engagement) | High (clarity) |

---

## Part III: Titles Are Assertions

**Bad titles** (descriptive):
- "Results"
- "Main Findings"
- "Regression Output"
- "Data Description"

**Good titles** (assertive):
- "Treatment increased distance by 61 miles on average"
- "Pre-trends are flat across all specifications"
- "The effect is concentrated among low-income households"
- "CPS covers 95% of the target population, 2005-2020"

Every slide title should be a complete sentence that states what the audience should take away. If removed from the deck, the titles alone should tell the paper's story.

---

## Part IV: Narrative Structure

### Three-Act Arc
1. **Setup** (20%): Why should you care? What's the question?
2. **Confrontation** (60%): How did you answer it? What did you find?
3. **Resolution** (20%): What does it mean? What's next?

### Pyramid Principle
Lead with the conclusion, then provide supporting evidence. Academic audiences are impatient — don't make them wait 30 minutes for the punchline.

### Opening
First 90 seconds determine whether the audience pays attention. Start with:
- A surprising fact
- A policy puzzle
- A number that demands explanation

Do NOT start with: outline slides, literature review, or "Thank you for having me."

### Closing
End with your contribution, not "thank you" or "questions?" The last slide the audience sees should reinforce your main message.

---

## Part V: Visual Grammar

### Typography
- **Minimum body text:** 24pt (nothing below 18pt, ever)
- **Titles:** 28-32pt, bold
- **Slide numbers:** small, bottom corner
- **Font family:** Sans-serif for slides (unlike papers which use serif)

### White Space
White space is not wasted space — it is breathing room. A slide with 30% white space is more effective than one with 5%.

### Bullets
- Maximum 4 bullets per slide
- Each bullet: one line, one idea
- If you need more, split into two slides
- Avoid sub-bullets entirely

### Data Visualization
- Figures > Tables for talks (always)
- Axis labels must be readable from the back of the room
- Use color purposefully (highlight the treatment group, gray out the rest)
- Remove chartjunk: gridlines, borders, legends when unnecessary

### Mathematical Content
- Show key equations only (Euler equation, first-order condition, estimating equation)
- Build equations incrementally across slides if complex
- Color-code terms to link notation to concepts

---

## Part VI: The Warm Professional Palette

A cohesive color system for Beamer slides:

```latex
% --- Primary ---
\definecolor{DeepNavy}{HTML}{1B2A4A}      % Titles, headers
\definecolor{Teal}{HTML}{2A9D8F}          % Emphasis, highlights
\definecolor{WarmOrange}{HTML}{E76F51}    % Treatment/key results

% --- Secondary ---
\definecolor{SoftPurple}{HTML}{6C5B7B}    % Secondary emphasis
\definecolor{WarmGray}{HTML}{8D8D8D}      % De-emphasis, axes
\definecolor{LightGray}{HTML}{F0F0F0}     % Backgrounds

% --- Accent ---
\definecolor{Cream}{HTML}{FAF3E0}         % Callout backgrounds
\definecolor{DeepRed}{HTML}{C44536}       % Warnings, negative results
\definecolor{Gold}{HTML}{F4A261}          % Positive results, benefits
\definecolor{SoftWhite}{HTML}{FEFEFE}     % Slide background
```

**Usage rules:**
- DeepNavy for all text and titles
- Teal for emphasis and hyperlinks
- WarmOrange for the treatment group or key finding
- WarmGray for control groups, axes, de-emphasized text
- Never use more than 3 colors on a single slide

---

## Part VII: Common Failures

| Failure | Fix |
|---------|-----|
| Text wall (>6 lines of text) | Cut to 3-4 bullets or split slide |
| Burying the lede | Lead with the result, then explain |
| Table on slide | Convert to figure or simplify drastically |
| Reading slides aloud | Slides show evidence; you tell the story |
| Agenda/outline slides | Cut them — start with the hook |
| "Thank you" as last slide | End on your contribution |
| Tiny font on tables | Minimum 18pt; simplify or move to appendix |
| Too many backup slides | 5-10 max; anticipate the top questions |

---

## Part VIII: Process

1. **Read the paper** — identify the 3 things the audience must remember
2. **Write slide titles first** — if titles alone don't tell the story, restructure
3. **Build key slide** — the single most important result gets the most design attention
4. **Build outward** — from key slide to motivation and conclusion
5. **Cut** — remove everything that doesn't serve the 3 takeaways
6. **Compile and check** — zero warnings, zero overfull boxes
7. **Practice aloud** — timing reveals what to cut
8. **Revise** — after practice, cut 20% more

---

## References

- Shklovsky, V. (1917). "Art as Device." (defamiliarization principle)
- Duarte, N. (2010). *Resonate.* (narrative structure)
- Reynolds, G. (2011). *Presentation Zen.* (visual design)
- Tufte, E. (2001). *The Visual Display of Quantitative Information.* (data viz)
- Shapiro, J. (2023). "How to Give an Applied Micro Talk." (economics-specific)
