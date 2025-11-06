```markdown
# Flam_RnD_Assignment

Curve parameter estimation for FLAM — estimating rotation (theta), exponential factor (M), and horizontal translation (X) for a parametric curve using Python. This README matches the exact steps and results in the provided Google Colab notebook `FLAM_Assignment.ipynb`. Decimal values are shown up to 4 digits.

## Table of contents
- [Overview](#overview)
- [Mathematical model](#mathematical-model)
- [Dataset](#dataset)
- [Files in this repository](#files-in-this-repository)
- [Environment & requirements](#environment--requirements)
- [Notebook summary](#notebook-summary)
- [Methodology (matching the notebook)](#methodology-matching-the-notebook)
- [Results (notebook values, 4-digit decimals)](#results-notebook-values-4-digit-decimals)
- [Final equations (substituted values)](#final-equations-substituted-values)
- [Evaluation metrics](#evaluation-metrics)
- [How to reproduce](#how-to-reproduce)
- [Notes & next steps](#notes--next-steps)
- [Contact](#contact)

## Overview
Given a CSV of (x,y) points sampled from an unknown parametric curve, the goal is to recover three model parameters:
- theta — rotation angle,
- M — exponential amplitude factor,
- X — horizontal translation.

The notebook assigns a uniform parameter t ∈ [6.0, 60.0] across all data points and fits the parametric model to the observed (x,y) coordinates.

## Mathematical model
The parametric curve used in the notebook is:

x(t) = t * cos(theta) - exp(M * |t|) * sin(0.3 * t) * sin(theta) + X

y(t) = 42 + t * sin(theta) + exp(M * |t|) * sin(0.3 * t) * cos(theta)

theta is used in radians inside the trig functions (the notebook converts degrees → radians).

## Dataset
- xy_data.csv — CSV file containing the observed (x, y) coordinates used for fitting.

Notebook-reported dataset details:
- Shape: (1500, 2)
- Assigned t: linspace(6.0, 60.0, 1500)

## Files in this repository
- xy_data.csv — Observed data points (x, y)
- FLAM_Assignment.ipynb — Google Colab notebook with data loading, parameter estimation, optimization and visualizations
- README.md — This document (updated to reflect the notebook)

## Environment & requirements
Recommended environment:
- Python 3.8+
- numpy
- pandas
- scipy
- matplotlib
- jupyter / Google Colab (notebook provided)

## Notebook summary
1. Load `xy_data.csv` into pandas and display shape (1500×2).
2. Scatter-plot raw (x,y) to inspect the curve.
3. Assign t = np.linspace(6.0, 60.0, N).
4. Grid-search theta (degrees) and compute closed-form X for each theta to get an initial theta/X.
5. Rotate coordinates to (u,v); estimate M by linear regression on log(amplitude) vs t using v and sin(0.3 t).
6. Use [theta, M, X] as initial guess and refine all three with scipy.optimize.minimize (bounded).
7. Plot fitted curve over data and compute L1 error.

## Methodology (matching the notebook)
- t is uniformly assigned: t = np.linspace(6.0, 60.0, N).
- Theta & X (initial): grid search over theta degrees in [0.01, 49.99] (1000 points). For each theta, X_hat is computed in closed form and MSE_u is used to pick the best theta.
- Rotation to local coordinates:
  u = (x - X_hat) * cos(theta) + (y - 42) * sin(theta)
  v = -(x - X_hat) * sin(theta) + (y - 42) * cos(theta)
- M estimation: for indices where sin(0.3 t) ≠ 0,
  log(|v|) - log(|sin(0.3 t)|) ≈ M * t + const, solved by least squares.
- Joint refinement: minimize mean squared error of (x,y) predictions with bounds:
  theta ∈ [0, 50] (degrees), M ∈ [-0.05, 0.05], X ∈ [0, 100].

## Results (notebook values, 4-digit decimals)
Intermediate and final numeric values taken directly from the executed Colab cells, rounded to 4 decimal digits:

- Dataset shape: (1500, 2)
- t assigned: 6.0 to 60.0 (1500 points)

Initial grid-search step:
- Best theta (initial grid): 0.0100°  
- Initial X (from grid): 50.7168  
- MSE_u (for that theta): 452.9082

Linear fit for M (using rotated coordinates from initial theta/X):
- Estimated M (linear fit): -0.0055  
- Intercept c0: 3.5005

Joint optimization (scipy.optimize.minimize) — refined parameters:
- Refined theta: 29.5828°  
- Refined theta (radians): 0.5163 rad  
- Refined M: -0.0500  
- Refined X: 55.0136  
- Final objective (mean squared error used in notebook): 514.4579

Evaluation metric:
- L1 distance between predicted and actual curve: 25.4015

> Note: the final M reached the optimizer lower bound (-0.0500). If a different M is expected, widen the bound and re-run the notebook.

## Final equations (substituted values)
Using theta = 0.5163 rad (29.5828°), M = -0.0500, X = 55.0136:

x(t) = t * cos(0.5163) - exp(-0.0500 * |t|) * sin(0.3 * t) * sin(0.5163) + 55.0136

y(t) = 42 + t * sin(0.5163) + exp(-0.0500 * |t|) * sin(0.3 * t) * cos(0.5163)

Desmos-friendly equation:
(t * cos(0.5163) - e^(-0.0500 * abs(t)) * sin(0.3 * t) * sin(0.5163) + 55.0136, 42 + t * sin(0.5163) + e^(-0.0500 * abs(t)) * sin(0.3 * t) * cos(0.5163))

## Evaluation metrics
- Final mean-squared objective (notebook): 514.4579  
- Final L1 (mean absolute deviation between predicted & actual x,y): 25.4015

## How to reproduce
1. Open `FLAM_Assignment.ipynb` in Google Colab (recommended).
2. Upload `xy_data.csv` (notebook includes a files.upload() cell).
3. Run all cells in order; the notebook prints intermediate estimates and plots the fitted curve.
4. To explore alternate fits, adjust `bounds` or `initial_guess` in the minimize call (cell with `scipy.optimize.minimize`).

## Notes & next steps
- The notebook uses a hybrid approach: grid-search for theta (closed-form X), linear regression for M, then joint constrained optimization. This is quick and effective given the model.
- Because the final M hit the specified lower bound (-0.0500), consider widening the M bounds if a more negative decay rate is plausible.
- For a more robust M estimate consider peak-picking the envelope of |v(t)| or using robust regression to reduce the impact of outliers.

```
