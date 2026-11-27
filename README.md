# Regularization: Selection, Shrinkage and Valid Intervals

## Objective

I examine how LASSO selection and shrinkage affect coefficient estimates and confidence intervals, and compare ways to separate prediction from inference.

## Methodology

- I simulate a treatment effect of 1.0 with five true confounders among 100 candidate controls.
- I select controls using LASSO, fit OLS on the treatment and selected controls, and measure interval coverage over 200 samples.
- I implement double selection using the union of controls selected for the outcome and treatment.
- I compare both procedures with OLS using only the five known confounders.
- I compare LASSO and post-LASSO OLS coefficients with known coefficients in a separate simulation.
- I complete a cross-validation loop over 31 penalties, fitting the scaler inside each training fold.
- I examine sample splitting in the optional extension.

## Key Findings

The analyst’s nominal 95% intervals cover the true effect in **82.5%** of the 200 samples. The mean estimate is **1.0400**, compared with the true effect of **1.0**. All five confounders are retained, so missing confounders cannot explain this result. Selecting additional controls on the same data still affects the estimate and its reported uncertainty.

Double selection improves coverage to **88.5%**, retaining **26.5 controls** on average. Its mean estimate is **0.9564**. It improves coverage here but does not guarantee exact finite-sample inference. OLS using only the five true confounders achieves **95.0%** coverage and a mean estimate of **0.9987**.

In the coefficient comparison, LASSO retains all **8 true predictors** and **11 false positives**. Mean proportional shrinkage among the true predictors is **4.6%**. The two smallest true coefficients experience the largest proportional shrinkage. Post-LASSO OLS removes the direct penalty, but does not automatically solve post-selection inference or establish causality.

My penalty-selection function matches GridSearchCV: **lambda = 0.0501**, with **CV MSE = 1.0173**. It also passes the checks using a reduced penalty grid and renamed columns on a smaller sample.

In the sample-splitting extension, the analyst’s procedure achieves **82.0%** coverage and misses at least one confounder in **43 of 200 samples**. The double-selection union achieves **94.0%** coverage and misses no confounders in these samples. Their mean interval widths are **0.4708** and **0.4859**, compared with **0.3206** for full-sample double selection. Splitting separates selection from estimation but leaves fewer observations for estimating the effect.

## Files

- `lab-ch16-diagnostic.ipynb`: executed notebook, written diagnoses, completed functions and optional extension.
- `src/sparse_tools.py`: support selection, double selection and coverage-study functions.
- `README.md`: project description and findings.
