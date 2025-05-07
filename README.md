# 📊 Statistical Inference and Density Estimation in R

This repository presents a comprehensive statistical modeling project written in **R with LaTeX integration** (`.Rnw`). It covers analytical derivation and simulation-based validation of:

- Mixture models using method of moments
- Kurtosis-based t-distribution fitting
- Kernel Density Estimation (KDE) with ISE minimization
- Skewed-t distribution maximum likelihood estimation (MLE) across 60+ years of S&P 500 returns

---

## 🗂 Project Files

| File | Description |
|------|-------------|
| `HW2_HaoChun_Shih.Rnw` | Main R + LaTeX source |
| `HW2_HaoChun_Shih.pdf` | Compiled report |
| `sp500_full.csv` | Historical S&P 500 daily return dataset |

---

## 📌 Topics Covered

### 1️⃣ Method of Moments for Normal Mixtures

- Derived:
  \[
  E[|X - \mu|] = p|c| + (1 - p)|-c| = |c|(2p - 1), \quad \text{Var}(X) = c^2
  \]
- Solved for \( \hat{p}, \hat{c}, \hat{\mu} \) via sample moments.

---

### 2️⃣ Kurtosis and Estimating \( \nu \) in t-Distribution

- Given sample kurtosis = 9, inverted closed-form kurtosis function of t-distribution:
  \[
  \text{kurt}(t_\nu) = \frac{6}{\nu - 4} + 3
  \Rightarrow \hat{\nu} = 5
  \]

---

### 3️⃣ Kernel Density Estimation & ISE Minimization

Simulated data from a 2-component Gaussian mixture and estimated density using different kernel bandwidths.

#### 📊 KDE Visualization at Different \( h \)
<img src="Images/KDE-1.png" width="500"/>

#### 📉 ISE Minimization to Find Optimal \( h^* \)
<img src="Images/ISEPlot-1.png" width="500"/>

#### 📐 KDE vs True Density at Optimal \( h = 0.25 \)
<img src="Images/PlotOptimization-1.png" width="500"/>

---

### 4️⃣ Skewed-t Distribution MLE on S&P 500 Returns (1962–2023)

- Estimated four parameters: \( \mu, \sigma, \nu, \xi \) with BFGS & Nelder-Mead
- Constructed 95% confidence intervals for each
- Identified:
  - 📉 **2020** had infinite variance estimate \( \nu < 2 \)
  - 📈 Strong skewness in 1984, 2007, 2014

#### 📊 Point Estimate & CI (BFGS)
<img src="Images/PlotNu-1.png" width="600"/>

#### 📊 Point Estimate & CI (Nelder-Mead)
<img src="Images/PlotNu-2.png" width="600"/>

#### 📈 Year 2020 Daily Returns (Infinite Variance)
<img src="Images/PlotSkewness-1.png" width="500"/>

#### 📈 KDE vs Fitted Skew-t (1984)
<img src="Images/PlotOptimization-2.png" width="500"/>

---

## 🧮 Sample Code

### Simulate 2-component Mixture Data
```r
set.seed(123)
n = 1000
p = 0.3
mu = 0
c = 2
mix = rbinom(n, 1, p)
X = mu + c * (2 * mix - 1) + rnorm(n)

m1 = mean(abs(X - mean(X)))
m2 = var(X)
c_hat = sqrt(m2)
p_hat = (m1 / c_hat + 1) / 2

ISE = function(h, x) {
  fhat = density(x, bw=h)$y
  ftrue = dnorm(x)
  return(mean((fhat - ftrue)^2))
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
