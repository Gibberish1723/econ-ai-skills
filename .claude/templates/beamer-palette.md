# Beamer Color Palette: Warm Professional

A cohesive color system for economics presentations. Used by the Storyteller agent.

---

## Color Definitions

```latex
% === Warm Professional Palette ===
% Paste into Beamer preamble

% --- Primary ---
\definecolor{DeepNavy}{HTML}{1B2A4A}      % Titles, headers, body text
\definecolor{Teal}{HTML}{2A9D8F}          % Emphasis, highlights, links
\definecolor{WarmOrange}{HTML}{E76F51}    % Treatment group, key results

% --- Secondary ---
\definecolor{SoftPurple}{HTML}{6C5B7B}    % Secondary emphasis
\definecolor{WarmGray}{HTML}{8D8D8D}      % De-emphasis, axes, gridlines
\definecolor{LightGray}{HTML}{F0F0F0}     % Backgrounds, shading

% --- Accent ---
\definecolor{Cream}{HTML}{FAF3E0}         % Callout box backgrounds
\definecolor{DeepRed}{HTML}{C44536}       % Warnings, negative results
\definecolor{Gold}{HTML}{F4A261}          % Positive results, benefits
\definecolor{SoftWhite}{HTML}{FEFEFE}     % Slide background
```

## Usage Rules

| Element | Color | Rationale |
|---------|-------|-----------|
| Slide titles | DeepNavy | Authority, readability |
| Body text | DeepNavy | Consistency |
| Emphasis text | Teal | Draws eye without alarm |
| Treatment/key result | WarmOrange | Stands out in figures and tables |
| Control group | WarmGray | De-emphasized by design |
| Hyperlinks | Teal | Conventional but warm |
| Positive findings | Gold | Warm, optimistic |
| Negative findings / warnings | DeepRed | Alert without panic |
| Callout boxes | Cream background | Soft contrast |
| Slide background | SoftWhite | Warmer than pure white |

## Maximum Colors Per Slide: 3

Never use more than 3 colors on a single slide (excluding background and body text). Typical combinations:
- DeepNavy + WarmOrange + WarmGray (treatment vs control)
- DeepNavy + Teal + Gold (emphasis + positive result)
- DeepNavy + DeepRed + WarmGray (warning + de-emphasis)

## ggplot2 Integration

```r
# Use palette in R figures
palette_warm <- c(
  "DeepNavy"    = "#1B2A4A",
  "Teal"        = "#2A9D8F",
  "WarmOrange"  = "#E76F51",
  "SoftPurple"  = "#6C5B7B",
  "WarmGray"    = "#8D8D8D",
  "Gold"        = "#F4A261",
  "DeepRed"     = "#C44536"
)

scale_color_warm <- function(...) {
  ggplot2::scale_color_manual(values = unname(palette_warm), ...)
}
scale_fill_warm <- function(...) {
  ggplot2::scale_fill_manual(values = unname(palette_warm), ...)
}
```
