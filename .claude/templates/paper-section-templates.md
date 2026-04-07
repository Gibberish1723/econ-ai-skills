# Paper Section Templates

Reference LaTeX patterns for economics paper sections. Used by the Writer agent.

---

## Introduction Template

```latex
\section{Introduction}\label{sec:intro}

% Hook: Start with a motivating fact, puzzle, or policy question
% [1 paragraph]

% Research question and contribution: What you do and why it matters
% State contribution clearly in first 2 pages
% [1-2 paragraphs]

% Preview of approach: Data, identification, key finding
% [1 paragraph]

% Literature positioning: How this advances the frontier
% [1-2 paragraphs]

% Roadmap: "The remainder of this paper is organized as follows..."
% [1 paragraph]
```

## Results Template

```latex
\section{Results}\label{sec:results}

% Lead with main specification result
% Report point estimate, standard error, and economic magnitude

\begin{table}[htbp]
\centering
\begin{threeparttable}
\caption{Main Results: Effect of Treatment on Outcome}\label{tab:main}
\input{tables/table1_main_results.tex}
\begin{tablenotes}
\footnotesize
\item \textit{Notes.} Unit of observation is [X]. All specifications include [controls/FE]. Standard errors clustered at [level] in parentheses. $^{***}p<0.01$, $^{**}p<0.05$, $^{*}p<0.1$.
\end{tablenotes}
\end{threeparttable}
\end{table}

% Interpret magnitude: "This corresponds to a [X]\% change relative to the mean of [Y]"
% Discuss statistical precision
% Present heterogeneity if illuminating mechanism
```

## Conclusion Template

```latex
\section{Conclusion}\label{sec:conclusion}

% Restate question and main finding [1 paragraph]
% Discuss mechanisms and interpretation [1 paragraph]
% Policy implications [1 paragraph]
% Limitations and caveats [1 paragraph]
% Future directions [1 paragraph, optional]
```

---

## Writing Tips

- **Introduction:** The first 2 pages must contain the contribution statement. Referees decide early.
- **Results:** Lead with the main result. Do not bury it after robustness checks.
- **Conclusion:** No new results. Restate, interpret, and point forward.
- **Throughout:** Report effect sizes with units (e.g., "a $500 increase" not "a significant effect").

Sources: Cochrane (2005), Shapiro (2023), Thomson (2011).
