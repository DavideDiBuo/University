# American Option Pricing with Longstaff–Schwartz Monte Carlo

**Master's Thesis · Quantitative Finance · Numerical Methods · Optimal Stopping**

A numerical study of **American option pricing** using the **Longstaff–Schwartz Least-Squares Monte Carlo (LSMC)** algorithm, with lattice-based benchmarks, variance-reduction methods, and systematic sensitivity analysis.

**Author:** Davide Di Buò  
**Institution:** University of Bologna  
**Degree:** M.Sc. in Statistical, Financial and Actuarial Sciences  
**Thesis supervisor:** Paolo Foschi

## Project overview

Unlike European options, American options can be exercised before maturity. Their valuation therefore involves an **optimal stopping problem**: at each exercise date, the holder must compare the immediate exercise payoff with the expected value of keeping the option alive.

This research studies how accurately and efficiently the Longstaff–Schwartz algorithm approximates that decision through least-squares regression on simulated asset-price paths. Numerical estimates are compared with **binomial and trinomial tree** benchmarks, with particular emphasis on American put options.

### Research objectives

- Implement and examine the backward-induction logic of LSMC under risk-neutral valuation.
- Assess sensitivity to the number of Monte Carlo paths, exercise dates, regression degree, and basis family.
- Compare polynomial, Laguerre, and Hermite regression bases.
- Investigate **antithetic variates**, **moment matching**, **Sobol quasi-Monte Carlo**, and **European-put control variates**.
- Evaluate pricing accuracy, standard errors, confidence intervals, RMSE across random seeds, and computational cost.
- Examine the exercise boundary across different moneyness, volatility, and maturity regimes.

## Mathematical framework

For an American option with exercise payoff $h(S_\tau)$, the arbitrage-free value is written as

$$
V_t = \sup_{\tau\in\mathcal{T}_{t,T}}
\mathbb{E}^{\mathbb{Q}}\left[e^{-r(\tau-t)}h(S_\tau)\mid\mathcal{F}_t\right],
$$

where $\tau$ is an admissible exercise time and $\mathbb{Q}$ is the risk-neutral measure.

Under the geometric Brownian motion model, the simulated underlying evolves according to

$$
S_{t+\Delta t}=S_t\exp\left[\left(r-q-\tfrac12\sigma^2\right)\Delta t+\sigma\sqrt{\Delta t}\,Z\right],
\qquad Z\sim\mathcal{N}(0,1).
$$

At each exercise date, the algorithm compares the **immediate payoff** with the **continuation value**:

$$
V(t,S_t) = \max\{h(S_t), C(t,S_t)\}
$$

The continuation value is the expected discounted future value of keeping the option alive:

$$
C(t,S_t) = \mathbb{E}^{\mathbb{Q}}\left[e^{-r\Delta t}V(t+\Delta t,S_{t+\Delta t})\mid S_t\right]
$$

LSMC approximates $C(t,S_t)$ by regressing discounted future cash flows on basis functions of the current simulated stock prices, proceeding **backward from maturity**. The exercise decision is then made path by path.

## Numerical methods

| Component | Methods examined |
|---|---|
| Underlying dynamics | Risk-neutral geometric Brownian motion |
| Reference pricing | Cox–Ross–Rubinstein binomial tree; trinomial tree |
| Core algorithm | Longstaff–Schwartz least-squares Monte Carlo |
| Regression bases | Laguerre, ordinary polynomial, Hermite |
| Sampling improvements | Antithetic variates, moment matching, Sobol QMC |
| Variance reduction | European put as a control variate |
| Combined specification | Enhanced LSMC (antithetic variates + moment matching) |
| Evaluation | Absolute/relative error, Monte Carlo SE, confidence intervals, multi-seed RMSE, runtime |

## Case studies and selected results

Five hypothetical market scenarios test the methods across distinct option characteristics. The table reports the **best specification by absolute pricing error in the main model comparison** (Thesis, Table 6.7); values are numerical simulation results rather than live market quotes.

| American put scenario | Best specification in main comparison | LSMC price | Trinomial benchmark | Absolute error |
|---|---|---:|---:|---:|
| ATM equity index | Enhanced LSMC | 6.0915 | 6.0896 | 0.0019 |
| ITM protective put after drawdown | Base LSMC (Laguerre) | 16.3802 | 16.3928 | 0.0126 |
| OTM crash protection | Enhanced LSMC | 5.2085 | 5.2037 | 0.0048 |
| High-volatility single stock | Sobol QMC LSMC | 15.5590 | 15.5600 | 0.0010 |
| Long-maturity dividend-paying ETF | Enhanced LSMC | 11.2775 | 11.2785 | 0.0009 |

### Main findings

1. **LSMC produces estimates close to fine-grid trinomial benchmarks** across the five scenarios analyzed.
2. **No single algorithm dominates in all settings.** The best specification depends on moneyness, volatility, maturity, and the evaluation criterion.
3. **Variance reduction improves statistical precision**, but lowering Monte Carlo variance does not necessarily correct errors in the estimated exercise boundary.
4. **Increasing the number of paths reduces sampling uncertainty** without guaranteeing monotonic convergence of a particular price estimate to the benchmark.
5. **Multiple random seeds matter:** the most accurate specification in one run need not have the lowest RMSE across independent replications.

For example, in the path-count sensitivity experiment, the reported Monte Carlo standard error decreases from **0.0999 with 5,000 paths** to **0.0224 with 100,000 paths** (Thesis, Table 6.10).

## Thesis structure

1. **American option pricing:** derivatives, risk-neutral valuation, GBM, and optimal stopping.
2. **Lattice benchmarks:** binomial/trinomial trees and numerical convergence.
3. **Longstaff–Schwartz Monte Carlo:** continuation-value regression and backward exercise decisions.
4. **Baseline numerical analysis:** paths, exercise dates, regression complexity, and benchmark accuracy.
5. **LSMC enhancements:** variance-reduction and improved path-generation techniques.
6. **Case studies:** five market scenarios, model comparison, robustness, and exercise-boundary diagnostics.
7. **Conclusions and future developments.**

## Practical relevance

The project connects **stochastic simulation**, **statistical learning through regression**, and **derivatives valuation**. Its methods are relevant to quantitative research, pricing model development, model validation, and financial risk analytics, especially when early-exercise features prevent straightforward closed-form valuation.

## Scope and interpretation

- The analysis focuses on numerical experiments and primarily one-dimensional American put valuation.
- The trinomial reference is a **high-resolution numerical benchmark**, not an exact analytical American-option price.
- The reported estimates depend on each experiment's model assumptions, sampling design, and numerical configuration.
- This README summarizes the **thesis research**; it does not imply the presence of a particular runnable software package or repository folder structure.

## Reference

**Di Buò, Davide.** *Pricing di opzioni americane attraverso il metodo Longstaff-Schwartz Monte Carlo*. Master's thesis, University of Bologna, M.Sc. in Statistical, Financial and Actuarial Sciences. Supervisor: Paolo Foschi.

---

*Academic research project. Not investment advice.*
