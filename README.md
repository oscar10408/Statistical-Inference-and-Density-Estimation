# 📊 Statistical Inference and Density Estimation in R

This repository contains an advanced statistical computing assignment written in **R and LaTeX (`.Rnw`)**, focusing on theoretical derivation and simulation-based validation. Topics include **method of moments**, **kurtosis of t-distributions**, **kernel density estimation**, **ISE minimization**, and **MLE for skewed-t models** across 60+ years of S&P 500 daily return data.

> 🧠 Built as part of EECS 545 coursework at the University of Michigan.

---

## 📦 Project Structure

| File | Description |
|------|-------------|
| `HW2_HaoChun_Shih.Rnw` | Main R + LaTeX source file |
| `HW2_HaoChun_Shih.pdf` | Rendered final report |
| `sp500_full.csv` | Historical daily S&P 500 data |

---

## 🧪 Key Topics Covered

### 1️⃣ Mixture Model Moment Derivation
- Derived formulas for \( E[|X - \mu|] \) and \( E[(X - \mu)^2] \) in a 2-component normal mixture model.
- Constructed **method-of-moments estimators** for \( p \), \( c \), and \( \mu \).

### 2️⃣ Estimating Degrees of Freedom via Kurtosis
- Computed analytical kurtosis of a **t-distribution**.
- Given sample kurtosis = 9, derived \( \hat{\nu} = 5 \) as the best-fit degrees of freedom.

### 3️⃣ Kernel Density Estimation (KDE) and ISE
- Simulated Gaussian mixture data and evaluated **KDE with various bandwidths**.
- Derived closed-form formula for **Integrated Squared Error (ISE)**.
- Identified optimal bandwidth \( h^* = 0.25 \) minimizing ISE.

📈 KDE vs True Distribution:
![KDE Plot](https://user-images.githubusercontent.com/your_id/kde_ise_plot.png)

---

## 📉 Auto-Correlation Analysis of Returns

Analyzed lag-based autocorrelations of S&P 500 returns and squared returns. While raw returns had little persistence, squared returns showed strong autocorrelation, consistent with volatility clustering.

#### ACF of Returns:
<img src="ACF-1.png" width="600"/>

#### ACF of Squared Returns:
<img src="ACF-2.png" width="600"/>

---

## 📐 Q-Q Plot Comparisons

### Normal vs t-distribution fit:
<img src="qqplots-1.png" width="600"/>

### Model Fitting via t-distribution (ν = 3, 4.2, 4.4):
<img src="model_fitting_via_qqplots.png" width="600"/>

---

## 📈 Historical Return Patterns

### S&P 500 Daily Returns (1962–2023)
<img src="S&P500_Returns.png" width="600"/>

### TSLA & NVDA Return Comparison (2022–2024)
<img src="Stock_Returns.png" width="600"/>

---

## 🔍 MLE for Skewed t-Distribution (1962–2023)

- Estimated \( \mu, \sigma, \nu, \xi \) using `optim()` with **BFGS** and **Nelder-Mead** methods.
- Plotted **point estimates with 95% confidence intervals** across years.
- Found **2020** exhibits **infinite variance** (\( \nu < 2 \)), likely due to COVID-19 market shock.
- Identified **significant skewness** in 1965, 1982, 1984, 1985, 2007, 2014.

📊 Visualization of infinite-variance year:
![2020 Returns](https://user-images.githubusercontent.com/your_id/infinite_variance_returns.png)

📊 Kernel vs fitted skew-t distribution (1984):
![Skewed t KDE](https://user-images.githubusercontent.com/your_id/skew_t_kde_1984.png)

---

## 🧠 Key Conclusions

- ✅ **Method of moments** yields accurate estimates with simulation validation.
- ✅ **ISE-based KDE** demonstrates bandwidth selection via theory-driven optimization.
- ✅ **Skewed-t fitting** with BFGS is more numerically stable than Nelder-Mead.
- ⚠️ **Years with \( \nu < 2 \)** indicate **infinite-variance behavior**, especially 2020.
- 🎯 Multiple years show **statistically significant skewness**, informing risk modeling.

---

## 💻 Reproducibility

To compile this project:

1. Open `HW2_HaoChun_Shih.Rnw` in RStudio
2. Ensure knitr is selected in: `Tools → Global Options → Sweave → Weave Rnw files using knitr`
3. Click "Knit to PDF"

Required packages:
```r
install.packages(c("knitr", "ggplot2", "dplyr", "xts", "MASS"))
