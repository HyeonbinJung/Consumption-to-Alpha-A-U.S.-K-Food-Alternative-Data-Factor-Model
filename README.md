<div align="center">

# 🍜 Signals to Alpha

**머신러닝 / 알파 팩터를 통한 경제, 산업 분석**<br>
<sub>Machine learning and alpha-factor research for economic and industry analysis</sub>

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" height="26">
<img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" height="26">
<img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" height="26">
<img src="https://img.shields.io/badge/statsmodels-4051B5?style=for-the-badge" height="26">
<img src="https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white" height="26">
<img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" height="26">
<img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" height="26">

[Overview](#-overview) · [Studies](#-studies) · [Method](#-method--k-food-study) · [Getting Started](#-getting-started) · [Structure](#%EF%B8%8F-project-structure) · [Notes](#-notes--limitations)

</div>

---

## 📌 Overview

A collection of research notebooks that turn **real-economy signals** — trade flows, consumer demand, industry data — into **testable alpha factors**, with an emphasis on honest evaluation: point-in-time construction, placebo tests and bias audits rather than headline backtests.

## 📚 Studies

| Study | Question | Status |
|---|---|---|
| [**Consumption-to-Alpha**](Consumption-to-Alpha-A-U.S.-K-Food-Alternative-Data-Factor-Model/) — a U.S. K-Food alternative-data factor model | Can K-food demand at *t* predict revenue growth and excess equity returns at *t + 1*? | Research-grade pilot (v3) |
| [**Revenue Dip Lab**](Revenue-Dip-Lab/) — fundamental resilience & price-drawdown backtesting engine | Do companies with resilient revenue offer systematic opportunities after sharp price declines? | Interactive platform · rule-based backtests |

## 🔬 Method — K-Food Study

| Step | What it does |
|---|---|
| 🧾 **Data hierarchy** | Sources graded A (government / company filings) · B (institutional databases) · C (press, descriptive only) |
| ⏱️ **Point-in-time signal** | `Signal = tanh(YoY growth / 20)` — a fixed transform, no full-sample z-scores |
| 🗺️ **Exposure map** | Company demand = Σ category exposure × category signal (ramen, kimchi, rice, sauce, broad) |
| 📈 **Equity lead test** | Demand score vs next-year return minus KOSPI (rank IC, long-short spread) |
| 🏭 **Fundamental lead test** | Demand score vs next-year sales growth (CJ uses Food-division sales) |
| 🧪 **Robustness** | Samyang removal, sauce exclusion, momentum control, 1,000 exposure-weight perturbations |
| 🎲 **Permutation placebo** | Shuffles scores across companies within each year |
| 🔍 **Bias audit** | Look-ahead, survivorship and exposure look-ahead checks |

**Verdict.** The economic thesis is credible and the **fundamental** signal is more promising than the equity factor, but five annual periods are **not enough to claim sector-wide alpha** — results strengthen after 2022 and remain regime-sensitive.

## 🧰 Tech Stack

| Layer | Tools |
|---|---|
| Analysis | Python, Jupyter, pandas, NumPy |
| Statistics | statsmodels (OLS), SciPy (Spearman / Pearson), permutation tests |
| Visualization | Matplotlib, browser charts and 3D CAGR surfaces |
| Platforms | FastAPI, shared JavaScript backtest engine, Web Workers, SQLite |
| Data | MAFRA, Korea Customs Service, company IR and filings, FRED / IMF, MarketScreener, SEC EDGAR, Yahoo Finance, Financial Modeling Prep |

## 🚀 Getting Started

```bash
pip install pandas numpy scipy statsmodels matplotlib jupyter
cd Consumption-to-Alpha-A-U.S.-K-Food-Alternative-Data-Factor-Model
jupyter notebook kfood_research_grade_alpha_v3.ipynb
```

## 🗂️ Project Structure

```text
Signals-to-Alpha
├── Revenue-Dip-Lab/                               backtesting platform — see its README
└── Consumption-to-Alpha-A-U.S.-K-Food-Alternative-Data-Factor-Model/
    ├── kfood_research_grade_alpha_v3.ipynb        research notebook (v3)
    ├── kfood_research_grade_panel.csv             demand score → next-year return panel
    ├── kfood_research_grade_fundamental_panel.csv demand score → next-year sales panel
    ├── kfood_exposure_matrix_v3.csv               company × category exposure priors
    └── kfood_source_register_v3.csv               data sources, coverage and quality grade
```

## 📝 Notes & Limitations

- Annual customs data are finalized after year-end, so the equity test is a **lead-association test**, not an executable January-1 backtest.
- Exposure weights are **analyst priors** informed by public product mix, not reported category revenue shares.
- The company universe is selected ex post (survivorship). H Mart appears only as a structural distribution proxy from press reporting.
- Research only — not investment advice.

---

<div align="center"><sub>Built by <a href="https://github.com/HyeonbinJung">Hyeonbin Jung</a></sub></div>
