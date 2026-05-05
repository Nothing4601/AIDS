# AE 248 — Artificial Intelligence and  Data Science (AIDS)

Course assignments and final project notebooks 

---

## Assignments

### AIDS-1 — ODI Batsman Score Analysis
Comparative statistical analysis of ODI cricket scores for 3 batsmen of choice.
- Boxplot and violin plots to compare score distributions side by side
- Error bar plot using `plt.errorbar` showing mean ± std and median ± IQR for each batsman
- Approximate PMF computed using `np.histogram`
- Probability that each batsman scores ≥ 50 and ≥ 100; conditional probability of scoring 50+ given the batsman crosses 10
- Final recommendation on which batsman to pick for the team, backed by the above analysis

---

### AIDS-2 — Central Limit Theorem Demonstration
Simulation-based verification of the Central Limit Theorem using a mixed population.
- A population of 1 million individuals — 50% mahouts (Normal, μ = 65 kg, σ = 5 kg) and 50% elephants (Normal, μ = 3350 kg, σ = 500 kg) — is created and visualized
- Samples of increasing size (n = 5 to 100) are drawn repeatedly; sample means are stored and their distributions plotted as histograms
- As n increases, the distribution of sample means converges to a normal distribution, demonstrating CLT empirically
- Variance of sample means plotted against n and compared against the theoretical σ²/n curve

---

### AIDS-3 — Confidence Intervals via Bootstrapping
Probabilistic analysis of cricket scores using bootstrapping.
- 80% confidence interval for Kohli's highest score in a 3-match series, estimated from his historical ODI data
- Three batsmen simulated playing together in a 3-match series; bootstrap sampling used to estimate combined total runs
- 80% confidence interval computed for the maximum combined score across the series

---

### AIDS-4 — Bayesian Inference for a Biased Coin
Posterior estimation using Bayes' theorem applied to coin-flip data.
- Reproduced the textbook posterior computation and compared results for different values of N (number of flips)
- Prior changed to a uniform distribution between 0.4 and 0.6; posterior evaluated with 5, 10, and 20 samples — limitations of this informative prior are discussed and a fix is proposed
- A biased coin with a known bias is simulated; the Bayesian approach is used to detect the bias by computing and plotting the posterior, along with the Bayes estimator of the bias probability p

---

### AIDS-5 — ESPN Cricinfo Data Scraping & Pandas Analysis
End-to-end data pipeline from web scraping to comparative batsman analysis.
- A general function written to parse any ODI batsman's career innings data from ESPN Cricinfo — accepts either a local HTML file or clipboard input, returns a clean and correctly typed DataFrame
- `df.info()` used to verify the DataFrame structure and dtypes
- Data scraped for 3 top-order batsmen and saved as CSV files
- Pandas-based analysis comparing batting averages, strike rates, and consistency across the three batsmen to determine the best performer
- Deep-dive on one batsman: effect of batting position on total score and strike rate; effect of innings number on average score; dismissal patterns compared between their best and worst performing opponent countries

---

## Project — Drone Onboard Multi-Modal Sensor Dataset

**Dataset:** [Zenodo Record #13682870](https://zenodo.org/records/13682870) — KIOS Centre of Excellence, University of Cyprus  
**Drone:** DJI Matrice 300 RTK · 20 flights · 10 Hz logging · 276,057 observations · 34 features  
**Flight states:** IDLE_HOVER, ASCEND, TURN, DESCEND, HMSL  
**Presentation:** [Google Drive](https://drive.google.com/drive/folders/1j5uvsmQ_jtCfpFjExxYDJNSHH2x5aizK?usp=sharing)

### Key Variables Studied
- **wind_speed** (m/s) — TriSonica Mini Wind & Weather Sensor
- **battery_voltage** (mV) — live battery telemetry during flight
- Supporting: altitude, speed, power consumption, angular velocity, linear acceleration, flight state labels

### Analysis

**Distribution Fitting**
- Speed and power are bimodal — shaped by flight state, not a single parametric distribution
- Linear acceleration and angular speed follow a **Lognormal** distribution, consistent with the physics of steady flight punctuated by rare high-intensity manoeuvres

**Hypothesis Testing**
| Hypothesis | Test | Result |
|---|---|---|
| Ascent speed > Descent speed | Mann-Whitney U | Confirmed (p ≈ 1.13 × 10⁻⁶³) |
| Power differs across all 5 flight states | One-way ANOVA | Confirmed (F = 17655, p ≈ 0); ASCEND draws most, IDLE_HOVER least |
| Angular speed higher during TURN than cruise | Mann-Whitney U | Confirmed (p ≈ 0) |

**Regression**
- **Power Model:** Multiple linear regression on altitude + speed + wind speed → R² = 0.577; all three predictors statistically significant
- **Battery Discharge:** Exponential decay V(t) = V₀·e^(−kt) fit using `scipy.optimize.curve_fit` (equivalent to log-linearisation); R² = 0.87 across all flights; V₀ = 48,973 mV, k = 7.75 × 10⁻⁵ s⁻¹ with tight 95% CIs

**Time-Series (ACF / PACF)**
- Strong autocorrelation present in all sensor streams, expected for 10 Hz continuous logging

### Conclusion
The analysis is consistent with the underlying physics of drone flight — states that demand more thrust consume more power, sensor statistics are non-Gaussian and state-dependent, and readings are internally consistent across 20 repeated flights.

---

## Stack

`numpy` · `pandas` · `matplotlib` · `seaborn` · `scipy` · `scikit-learn` · `statsmodels`

---

> **Note:** The raw dataset (`drone_data.csv`, ~72 MB) is not included in this repository. Download it from [Zenodo #13682870](https://zenodo.org/records/13682870) and place it in the root directory before running `Project.ipynb`.
