# LaTeX Economics Commands Library

Custom LaTeX commands for economic models and theory sections. Used by the Writer agent.

---

## Custom Commands (add to preamble)

```latex
% --- Operators ---
\DeclareMathOperator*{\argmax}{arg\,max}
\DeclareMathOperator*{\argmin}{arg\,min}
\newcommand{\E}{\mathbb{E}}           % Expectations
\newcommand{\Var}{\text{Var}}          % Variance
\newcommand{\Cov}{\text{Cov}}         % Covariance
\newcommand{\Corr}{\text{Corr}}       % Correlation
\newcommand{\plim}{\text{plim}}       % Probability limit

% --- Derivatives ---
\newcommand{\pd}[2]{\frac{\partial #1}{\partial #2}}      % Partial derivative
\newcommand{\dd}[2]{\frac{d #1}{d #2}}                    % Total derivative
\newcommand{\pdd}[2]{\frac{\partial^2 #1}{\partial #2^2}} % Second partial

% --- Sets and Spaces ---
\newcommand{\R}{\mathbb{R}}           % Real numbers
\newcommand{\N}{\mathbb{N}}           % Natural numbers
\newcommand{\Z}{\mathbb{Z}}           % Integers

% --- Econometrics ---
\newcommand{\iid}{\stackrel{\text{iid}}{\sim}}
\newcommand{\pto}{\xrightarrow{p}}    % Convergence in probability
\newcommand{\dto}{\xrightarrow{d}}    % Convergence in distribution
\newcommand{\asto}{\xrightarrow{a.s.}} % Almost sure convergence
```

## Theorem Environments

```latex
\usepackage{amsthm}

\newtheorem{theorem}{Theorem}[section]
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{corollary}[theorem]{Corollary}
\newtheorem{definition}{Definition}[section]
\newtheorem{assumption}{Assumption}
\newtheorem{remark}{Remark}[section]
```

## Consumer Optimization Template

```latex
\begin{align}
\max_{c_1, c_2} \quad & u(c_1) + \beta u(c_2) \label{eq:objective} \\
\text{s.t.} \quad & c_1 + \frac{c_2}{1+r} = y_1 + \frac{y_2}{1+r} \label{eq:budget}
\end{align}

The Euler equation (first-order condition) is:
\begin{equation}
u'(c_1) = \beta (1+r) u'(c_2) \label{eq:euler}
\end{equation}
```

## Bellman Equation Template

```latex
\begin{equation}
V(k) = \max_{k'} \left\{ u(f(k) - k') + \beta V(k') \right\} \label{eq:bellman}
\end{equation}

subject to $0 \leq k' \leq f(k)$, where $f(k) = k^\alpha$ is the production function.
```

## Notation Conventions

| Symbol | Meaning | Anti-pattern |
|--------|---------|-------------|
| $Y_{it}$ | Outcome for unit $i$ at time $t$ | Don't use $y$ without subscripts |
| $D_{it}$ | Treatment indicator | Don't use $T$ (conflicts with time) |
| $\beta$ | Coefficient of interest | State what it identifies |
| $\varepsilon_{it}$ | Error term | Don't use $e$ (conflicts with Euler's number) |
| $X_{it}$ | Controls vector | Always define components |
