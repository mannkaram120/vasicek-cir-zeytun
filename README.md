# Vasicek & CIR Short-Rate Models
### Replication & Extension of Zeytun & Gupta (2007)

A complete Python replication of **Zeytun & Gupta (2007)** extended with three original contributions — a direct comparison of two calibration methods, a cross-country parameter stability analysis across Canada, USA, and EUR, and application to 30 years of market data covering five major macroeconomic regimes.

> **Article:** Full write-up with analysis and charts published on Medium — [link coming soon]

---

## Paper Replicated

Zeytun, S. & Gupta, A. (2007).
**A Comparative Study of the Vasicek and the CIR Model of the Short Rate.**
Fraunhofer ITWM Technical Report 124.
[Free PDF — Fraunhofer ITWM](https://www.itwm.fraunhofer.de)

---

## Repository Structure

```
vasicek-cir-zeytun/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   └── zeytun_replication.ipynb
│
├── figures/
│   ├── fig01_data_overview.png
│   ├── fig02_model_simulations.png
│   ├── fig03_affine_structure.png
│   ├── fig04_sensitivity.png
│   ├── fig05_method1_calibration.png
│   ├── fig06_method1_fit.png
│   ├── fig07_method2_results.png
│   ├── fig08_method_comparison.png
│   ├── fig09_stability_cv.png
│   ├── fig10_rolling_stability.png
│   ├── fig11_cross_country_params.png
│   └── fig12_full_period_extension.png
│
└── data/
    └── README_data.md
```

---

## Notebook Structure

`zeytun_replication.ipynb` — 11 sections, self-contained, no external engine file.

| # | Section | Type |
|---|---------|------|
| 1 | Data loading — Canada, USA, EUR, CORRA | Setup |
| 2 | Model definitions — Vasicek & CIR SDEs with simulation | Replication |
| 3 | Affine term structure — bond pricing, B(t,T), convexity property | Replication |
| 4 | Sensitivity analysis — effect of κ, θ, σ on paths and bond prices | Replication |
| 5 | Calibration Method 1 — bond price least-squares fitting | Replication |
| 6 | Calibration Method 2 — Phillips-Yu estimator + Girsanov theorem | Replication |
| 7 | Method 1 vs Method 2 — direct comparison | **Original** |
| 8 | Parameter stability study — CV analysis across regimes | **Original** |
| 9 | Cross-country test — Canada vs USA vs EUR | **Original** |
| 10 | Full period 1997–2026 — dot-com, GFC, COVID, rate hikes | **Original** |
| 11 | Summary and conclusions | — |

---

## Key Findings

**1 — CIR σ is more stable than Vasicek σ across all three markets**

Contradicts Zeytun's original conclusion. The Feller condition (2κθ > σ²) acts as an implicit regulariser, constraining CIR parameters within economically plausible ranges.

| Country | Vasicek σ CV | CIR σ CV | More Stable |
|---------|-------------|---------|-------------|
| Canada | 168% | 79% | CIR |
| USA | 158% | 83% | CIR |
| EUR | 251% | 74% | CIR |

**2 — Two calibration methods serve different purposes**

| Method | Approach | RMSE | σ Stability | Best used for |
|--------|---------|------|------------|---------------|
| Method 1 | Bond price fitting | 0.065% | CV = 88% | Derivative pricing |
| Method 2 | Phillips-Yu + Girsanov | 0.389% | CV = 6% | Risk management, stress testing |

Method 2 produces parameters 14.7× more stable despite its higher RMSE.

**3 — Market price of risk λ is economically large**

Vasicek λ averaged 4.71 vs CIR λ of 1.80 on Canadian data. The large Vasicek λ reflects a structural difficulty reconciling real-world rates (~3%) with market-implied equilibrium (~7–9%) during 2005–2006.

**4 — EUR negative rates break Vasicek stability**

Post-2014 EUR Vasicek σ CV = 251% vs CIR σ CV = 74%. CIR is substantially more resilient in negative rate environments due to the rate-shifting procedure.

**5 — θ evolution through five market regimes (1997–2026)**

Long-run mean θ collapsed to 1.4% at the zero lower bound post-COVID, then recovered sharply to 7%+ after the 2022 rate hike cycle — the fastest hiking cycle in 40 years. Neither Vasicek nor CIR had been applied to this environment in the original paper.

---

## Data

All four datasets are free from official sources. Data files are **not included** due to licensing. See `data/README_data.md` for exact download instructions.

| File | Source | Period | Maturities |
|------|--------|--------|-----------|
| `yield_curves.csv` | Bank of Canada | 1997–2026 | 14 (0.25Y–30Y) |
| `CORRA.csv` | Bank of Canada | 1997–2026 | Overnight rate |
| `FRED_treasury_rates_merged.csv` | Federal Reserve (FRED) | 1997–2026 | 10 (3M–30Y) |
| `ECB_Data_Portal_*.csv` | ECB Data Portal | 2004–2026 | 7+ (1Y–30Y) |

---

## Quick Start

```bash
# 1. Clone
git clone https://github.com/yourusername/vasicek-cir-zeytun.git
cd vasicek-cir-zeytun

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download data
# Follow instructions in data/README_data.md
# Place all four CSV files in the data/ folder

# 4. Launch
jupyter notebook notebooks/zeytun_replication.ipynb
```

Add this as the **first code cell** and update the path to match your machine:

```python
import os
os.chdir(r"path\to\vasicek-cir-zeytun")
print("Working directory:", os.getcwd())
print("Data files:", os.listdir('data'))
```

**Estimated runtimes:**
- Sections 1–4: under 1 minute
- Section 5 (Method 1 calibration — 120 months): ~10 minutes
- Section 6 (Method 2 — 17 rolling windows): ~5 minutes
- Section 9 (Cross-country — 967 months total): ~25 minutes

---

## Tools & Libraries

| Library | Version | Purpose |
|---------|---------|---------|
| Python | 3.9+ | Core language |
| NumPy | ≥1.24 | Numerical computation, simulation |
| Pandas | ≥2.0 | Data management, resampling |
| SciPy | ≥1.10 | Optimisation (calibration), statistics |
| Matplotlib | ≥3.7 | All figures |
| Jupyter | ≥1.0 | Interactive notebook environment |

---

## References

Vasicek, O. (1977). An Equilibrium Characterization of the Term Structure.
*Journal of Financial Economics*, 5(2), 177–188.

Cox, J.C., Ingersoll, J.E., & Ross, S.A. (1985). A Theory of the Term Structure of Interest Rates.
*Econometrica*, 53(2), 385–407.

Phillips, P.C.B. & Yu, J. (2005). A Two-Stage Realized Volatility Approach to Estimation
for Diffusion Processes from Discrete Observations.
*Cowles Foundation Discussion Paper No. 1523*.

Zeytun, S. & Gupta, A. (2007). A Comparative Study of the Vasicek and the CIR Model
of the Short Rate. *Fraunhofer ITWM Technical Report 124*.

---

## Related

**Orlando, Mininni & Bufalo (2019) — Forecasting with partitioning**
Applies the same models to weekly EUR data using a regime-detection algorithm
and rolling-window forecaster. Separate repository — coming soon.
