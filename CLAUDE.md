# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an **econometrics research assignment** using Monte Carlo simulation to study finite-sample properties of statistical estimators and hypothesis tests under heteroscedasticity.

- **Language:** Python
- **Main file:** `claude-test/trabajo_econometria.ipynb`
- **Dependencies:** `numpy`, `scipy.stats` (no requirements file — assumed installed)

## Running the Notebook

```bash
jupyter notebook claude-test/trabajo_econometria.ipynb
```

Or run all cells non-interactively:

```bash
jupyter nbconvert --to notebook --execute claude-test/trabajo_econometria.ipynb --output output.ipynb
```

## Architecture

The notebook is structured in two independent research parts, each running **5000 Monte Carlo simulations** (`N_SIM = 5000`, `SEED = 1910`).

### Part 1: FGLS Properties

Studies finite-sample behavior of OLS, GLS, and FGLS estimators under group-wise heteroscedasticity.

- **Model:** `y = β₀ + β₁x + u`, with β₀ = −3.0, β₁ = 0.8, 5 groups with variances `[4, 9, 16, 25, 36]`
- **Key functions:** `generate_data()`, `ols()`, `gls()`, `fgls()`, `run_simulation_1a()`, `run_simulation_1b()`
- `fgls()` follows a 3-step procedure: OLS → estimate group variances → GLS transform
- `run_simulation_1a()` sweeps sample sizes `[5, 10, 30, 100, 200, 500]`; `run_simulation_1b()` compares GLS (Cholesky) vs. FGLS

### Part 2: White's Heteroscedasticity Test

Studies size and power of White's test under three designs:

- **Design 0:** Homoscedastic, normal errors
- **Design 1:** Heteroscedastic (`νᵢ = exp(0.25x₁ + 0.25x₂)`), normal errors
- **Design 2:** Heteroscedastic, non-normal errors (`t₅`)

- **Key functions:** `generate_x_part2()`, `generate_y_part2()`, `white_test()`, `run_white_test_simulation()`, `white_variance_simulation()`
- `white_test()` uses the auxiliary regression approach (regress squared OLS residuals on regressors, their squares, and cross-products)
- `white_variance_simulation()` computes relative bias of White's sandwich variance estimator vs. true errors
