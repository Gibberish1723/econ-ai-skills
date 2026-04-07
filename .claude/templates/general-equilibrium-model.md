# General Equilibrium Model Builder

Reference template for building and solving Walrasian GE models. Julia implementation.

**Author:** Abhimanyu Nag
**Scope:** Pure exchange economies (no production).

---

## When to Use

- Formalizing a pure exchange GE model for a paper
- Teaching microeconomic theory (Edgeworth box, welfare theorems)
- Numerically solving for equilibrium prices and allocations
- Comparative statics on endowment or preference changes

## Theoretical Framework

A **pure exchange economy** $\mathcal{E} = \{(u^i, \omega^i)_{i=1}^I\}$ where:
- $u^i: \mathbb{R}^L_+ \to \mathbb{R}$ is consumer $i$'s utility
- $\omega^i \in \mathbb{R}^L_+$ is consumer $i$'s endowment

**Consumer's Problem:** $\max_{x^i} u^i(x^i)$ s.t. $p \cdot x^i \leq p \cdot \omega^i$

**Walrasian Equilibrium:** $(x^*, p^*)$ such that all consumers optimize and markets clear: $\sum_i x^{*i} = \sum_i \omega^i$

**Key Theorems:** Walras' Law ($p \cdot z(p) = 0$), First Welfare Theorem (equilibrium → Pareto), Second Welfare Theorem (Pareto → equilibrium with transfers), Existence (Debreu 1959).

## Julia Implementation

### Requirements

```julia
using Pkg
Pkg.add(["NLsolve", "LinearAlgebra", "Plots", "ForwardDiff"])
```

### Economy Structure

```julia
using LinearAlgebra, NLsolve, Plots

struct PureExchangeEconomy
    n_goods::Int
    n_consumers::Int
    endowments::Matrix{Float64}   # I × L
    utility_params::Vector{Any}
    utility_type::Symbol           # :cobb_douglas, :ces, :leontief
end
```

### Demand and Excess Demand

```julia
function demand_cobb_douglas(p, wealth, α)
    α_normalized = α / sum(α)
    return α_normalized .* wealth ./ p
end

function excess_demand(p, economy::PureExchangeEconomy)
    z = zeros(economy.n_goods)
    for i in 1:economy.n_consumers
        ω_i = economy.endowments[i, :]
        wealth_i = dot(p, ω_i)
        if economy.utility_type == :cobb_douglas
            α_i = economy.utility_params[i]
            x_i = demand_cobb_douglas(p, wealth_i, α_i)
        end
        z += x_i - ω_i
    end
    return z
end
```

### Equilibrium Solver (Log-Price Parameterization)

```julia
function solve_equilibrium(economy::PureExchangeEconomy)
    p0 = zeros(economy.n_goods - 1)  # Log-space initial guess
    function excess_demand_reduced!(F, x)
        p_rest = exp.(x)
        p = vcat(1.0, p_rest)  # Numeraire p_1 = 1
        z = excess_demand(p, economy)
        F .= z[2:end]         # Walras' Law: good 1 clears automatically
    end
    result = nlsolve(excess_demand_reduced!, p0, autodiff=:forward)
    converged(result) || error("Solver did not converge")
    return vcat(1.0, exp.(result.zero))
end
```

### Complete Example (2×2 Economy)

```julia
economy = PureExchangeEconomy(
    2, 2,
    [4.0 1.0; 1.0 4.0],           # Endowments
    [[0.6, 0.4], [0.3, 0.7]],     # Cobb-Douglas params
    :cobb_douglas
)

p_star = solve_equilibrium(economy)
x_star = equilibrium_allocations(p_star, economy)

# Verify market clearing
@assert isapprox(sum(x_star, dims=1), sum(economy.endowments, dims=1))
```

### Edgeworth Box Visualization

```julia
function plot_edgeworth_box(economy, p_star, x_star)
    ω_total = vec(sum(economy.endowments, dims=1))
    ω1 = economy.endowments[1, :]
    wealth1 = dot(p_star, ω1)
    
    plt = plot(xlim=(0, ω_total[1]), ylim=(0, ω_total[2]),
               xlabel="Good 1", ylabel="Good 2", aspect_ratio=:equal)
    scatter!([ω1[1]], [ω1[2]], label="Endowment", markersize=8, color=:red)
    scatter!([x_star[1,1]], [x_star[1,2]], label="Equilibrium", markersize=8, color=:green)
    x1_range = range(0, ω_total[1], length=100)
    x2_budget = (wealth1 .- p_star[1] .* x1_range) ./ p_star[2]
    plot!(x1_range, x2_budget, label="Budget line", color=:blue, linewidth=2)
    return plt
end
```

## Numerical Best Practices

1. Always verify market clearing after solving
2. Check Walras' Law numerically ($p \cdot z \approx 0$)
3. Verify Pareto efficiency via MRS equality
4. Use multiple initial guesses if solver doesn't converge
5. Normalize prices (set $p_1 = 1$) to resolve scale indeterminacy

## References

- Mas-Colell, Whinston, and Green (1995). *Microeconomic Theory*. Chapters 15-17.
- Debreu (1959). *Theory of Value*. Yale University Press.
- Arrow & Debreu (1954). Existence of an equilibrium for a competitive economy. *Econometrica*.
- QuantEcon Julia lectures: https://julia.quantecon.org/
