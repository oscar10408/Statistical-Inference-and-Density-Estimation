# 📊 Statistical Inference and Density Estimation in R

This repository presents a comprehensive statistical modeling project written in **R with LaTeX integration** (`.Rnw`). It covers analytical derivation and simulation-based validation of:

- Mixture models using method of moments
- Kurtosis-based t-distribution fitting
- Kernel Density Estimation (KDE) with ISE minimization
- Skewed-t distribution maximum likelihood estimation (MLE) across 60+ years of S&P 500 returns

---

## 🧮 Sample Code

```r
set.seed(123)
n = 1000; p = 0.3; mu = 0; c = 2
mix = rbinom(n, 1, p)
X = mu + c * (2 * mix - 1) + rnorm(n)

m1 = mean(abs(X - mean(X)))
m2 = var(X)
c_hat = sqrt(m2)
p_hat = (m1 / c_hat + 1) / 2

ISE = function(h, x) {
  fhat = density(x, bw=h)$y
  ftrue = dnorm(x)
  mean((fhat - ftrue)^2)
}
opt_h = optimize(ISE, c(0.05, 0.8), x=X)$minimum

negloglik = function(par, x) {
  mu = par[1]; sigma = exp(par[2])
  nu = exp(par[3]); xi = par[4]
  loglik = -sum(log(dskewt(x, mu, sigma, nu, xi)))
  return(loglik)
}
optim(par=c(0, log(1), log(5), 1), fn=negloglik, x=returns)
```
---

## 🗂 Project Files
| File | Description |
|------|-------------|
| `Statistical-Inference-and-Density-Estimation.Rnw` | Main R + LaTeX source |
| `Statistical-Inference-and-Density-Estimation.pdf` | Compiled report |
| `sp500_full.csv` | Historical S&P 500 data |

---

## 📌 Topics Covered

### 1️⃣ Kernel Density Estimation & ISE Minimization

Simulated data from a 2-component Gaussian mixture and estimated density using different kernel bandwidths.

#### 📊 KDE Visualization at Different h
<img src="Images/KDE-1.png" width="500"/>

#### 📉 ISE Minimization to Find Optimal h*
<img src="Images/ISEPlot-1.png" width="500"/>

#### 📐 KDE vs True Density at Optimal h = 0.25
<img src="Images/PlotOptimization-1.png" width="500"/>

---

### 2️⃣ Skewed-t Distribution MLE on S&P 500 Returns (1962–2023)

Estimated four parameters: μ (mean), σ (scale), ν (degrees of freedom), and ξ (skewness) using both BFGS and Nelder-Mead optimization methods. Plotted 95% confidence intervals and parameter trajectories across decades of S&P 500 returns.

#### 📊 Point Estimate & CI (BFGS)
<img src="Images/PlotNu-1.png" width="600"/>

| Year | Method | ν     |
|------|--------|--------|
| 2020 | BFGS   | 1.9826 |
| 2020 | NM     | 1.9136 |

| Year | Method | ν     |
|------|--------|--------|
| 1987 | BFGS   | 2.4220 |
| 2008 | BFGS   | 2.3181 |
| 2010 | BFGS   | 2.8569 |
| 2016 | BFGS   | 2.8540 |
| 1987 | NM     | 2.3880 |
| 2008 | NM     | 2.3025 |
| 2010 | NM     | 2.7408 |
| 2016 | NM     | 2.6563 |
| 2018 | NM     | 2.9800 |



#### 📊 Point Estimate & CI (Nelder-Mead)
<img src="Images/PlotNu-2.png" width="600"/>

| Year | Method | ξ      | SE(ξ)   | z-value | p-value | Significant |
|------|--------|--------|---------|---------|---------|-------------|
| 1965 | BFGS   | 0.8341 | 0.0795  | -2.087  | 0.0369  | ✅           |
| 1982 | BFGS   | 1.2751 | 0.1175  |  2.341  | 0.0193  | ✅           |
| 1984 | BFGS   | 1.4201 | 0.1335  |  3.148  | 0.0016  | ✅           |
| 1985 | BFGS   | 1.2552 | 0.1263  |  2.021  | 0.0433  | ✅           |
| 2007 | BFGS   | 0.8598 | 0.0688  | -2.037  | 0.0416  | ✅           |
| 2014 | BFGS   | 0.8553 | 0.0718  | -2.015  | 0.0439  | ✅           |
| 1965 | NM     | 0.8295 | 0.0802  | -2.125  | 0.0336  | ✅           |
| 1982 | NM     | 1.2778 | 0.1187  |  2.340  | 0.0193  | ✅           |
| 1985 | NM     | 1.2692 | 0.1269  |  2.122  | 0.0339  | ✅           |
| 2007 | NM     | 0.8594 | 0.0694  | -2.027  | 0.0427  | ✅           |
| 2014 | NM     | 0.8513 | 0.0734  | -2.027  | 0.0427  | ✅           |

#### 📈 Year 2020 Daily Returns (Infinite Variance)
<img src="Images/PlotSkewness-1.png" width="500"/>

#### 📈 KDE vs Fitted Skew-t (1984)
<img src="Images/PlotOptimization-2.png" width="500"/>

---

## ✅ Key Findings & Conclusion

- 📌 **Method of Moments** successfully recovers μ, p, and c using absolute deviation and variance. Confidence intervals for estimators are derived via simulation (n = 500, N = 1000).
- 📌 **Kurtosis-based estimation** shows that for kurtosis = 9, the best-fitting t-distribution has ν = 5.
- 📌 **KDE bandwidth optimization** via ISE yields optimal h* = 0.25, balancing bias and variance in the mixture distribution.
- ⚠️ **2020 exhibits infinite variance** (both BFGS: ν = 1.98, NM: ν = 1.91) due to extreme COVID-19 market shocks.
- 📉 **Heavy-tailed years** include 1987 (Black Monday), 2008 (Global Financial Crisis), 2010 (Flash Crash), and 2016 (Geopolitical events).
- 🎯 **Statistically significant skewness** appears in 1965, 1982, 1984, 1985, 2007, and 2014. For example, in 1984, ξ̂ = 1.42, rejecting the null hypothesis H₀: ξ = 1 at the 5% level.
- 🧭 **BFGS optimizer is more stable** than Nelder-Mead across all parameter estimates, especially for estimating the degrees of freedom parameter ν.


---

## 💻 How to Compile

To reproduce the report:

1. Open `Statistical-Inference-and-Density-Estimation.Rnw` in **RStudio**
2. Set knitting engine to **knitr**
3. Click **Knit to PDF**
