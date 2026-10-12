<div align="center">

# 📉 Revenue Dip Lab

**Fundamental resilience & price-drawdown backtesting engine**

<sub>Do companies with resilient revenue offer systematic opportunities when their stock prices fall sharply?</sub>

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" height="26">
<img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" height="26">
<img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" height="26">
<img src="https://img.shields.io/badge/Web%20Workers-1F2937?style=for-the-badge" height="26">
<img src="https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" height="26">
<img src="https://img.shields.io/badge/SEC%20EDGAR-1F2937?style=for-the-badge" height="26">
<img src="https://img.shields.io/badge/Yahoo%20Finance-6001D2?style=for-the-badge&logo=yahoo&logoColor=white" height="26">

<sub><b>Program</b> Signals to Alpha &nbsp;·&nbsp; <b>Discipline</b> Quantitative equity research &nbsp;·&nbsp; <b>Methods</b> fundamental screening · drawdown signals · historical backtesting · performance attribution</sub>

[Overview](#-overview) · [Hypothesis](#-research-hypothesis) · [Methodology](#%EF%B8%8F-strategy-methodology) · [Features](#-key-features) · [Getting Started](#-getting-started) · [Structure](#%EF%B8%8F-project-structure) · [Notes](#-notes--limitations)

</div>

---

## 📌 Overview

Revenue Dip Lab is an interactive research platform for testing equity strategies built on the **divergence between market prices and corporate fundamentals**. Instead of relying only on price momentum or valuation multiples, it combines **revenue consistency** with **configurable price-drawdown signals**, then replays every trade on a shared clock so the result can be inspected, not just reported.

> **Model type.** A deterministic, rule-based strategy — not a machine-learning prediction model. Decisions come from explicitly defined fundamental and price conditions; parameter sweeps are used to study sensitivity, not to fit a model.

## 🧪 Research Hypothesis

| | |
|---|---|
| **H₀** | Buying companies with resilient revenue after substantial price declines does **not** generate statistically significant risk-adjusted excess returns. |
| **H₁** | Companies with stable or consistently growing revenue experience temporary price dislocations followed by positive subsequent returns. |

## ⚙️ Strategy Methodology

| Step | Rule (defaults, all configurable) |
|---|---|
| **1 · Universe** | Rank by market cap and take the top N (max 200); missing revenue never substitutes a lower-ranked stock |
| **2 · Fundamental screen** | Four annual revenues → three YoY growth rates; every rate ≥ 0% and `max − min ≤ 5 pp`. Only reports **published before** the signal date are used |
| **3 · Drawdown signal** | Close vs the close ~**30 calendar days** earlier; entry when the decline is ≥ **10%** |
| **4 · Position sizing** | `$10,000 + floor(decline% − 10) × $1,000`, sized at the signal close and filled at the next open; one entry per symbol, no leverage or shorting |
| **5 · Exit** | Take profit at **+10%** (next open) or sell at the holding deadline (**3 months**); **10 bps** one-way costs on each side |
| **6 · Evaluation** | CAGR, cumulative return, maximum drawdown, win rate, trade-level P&L, equity curve |

`CAGR = (Ending portfolio value / Beginning portfolio value)^(1 / years) − 1` — revenue CAGR is evaluated separately as a screening variable.

## ✨ Key Features

| Feature | Description |
|---|---|
| 🧭 **Five-panel workspace** | Strategy · Results · Price signals · CAGR surface · Factor search — side by side, draggable, resizable |
| 🎬 **Replayable backtests** | Equity curve and price charts draw on one clock; buy / sell markers appear as their execution dates pass |
| 🗻 **CAGR surface sweep** | Sweep any two parameters (up to 20,000 backtests) into a rotatable 3D CAGR surface, streamed from a Web Worker |
| 🔬 **Factor search with out-of-sample test** | All six parameters at once (random 5,000 or full grid up to 100,000) across four workers; η² sensitivity, parallel coordinates, in-sample vs out-of-sample Spearman ρ |
| 🗄️ **Real-data mode** | Yahoo Finance prices + SEC EDGAR revenue with original filing dates, cached in SQLite (2022–2025) |
| 🔌 **Optional FMP connector** | Financial Modeling Prep screener, prices and income statements — API key stays on the server |
| 📤 **Export** | Trades and sweep grids as CSV, datasets as JSON; JSON import for your own data |

> **Finding.** On the real 2022–2025 data, 5,000 random parameter combinations gave an in-sample vs out-of-sample CAGR rank correlation of **ρ = −0.56** — the in-sample optimum does **not** survive out of sample, a direct warning against picking the "best" cell.

## 🧰 Tech Stack

| Layer | Tools |
|---|---|
| Backend | Python, FastAPI, Uvicorn |
| Engine | Shared JavaScript backtest engine — runs identically in the browser and through the API (Node.js runner) |
| Frontend | Browser JavaScript, Web Workers, interactive charts and 3D surface |
| Data | SEC EDGAR companyfacts, Yahoo Finance (`yfinance`), Financial Modeling Prep (optional), SQLite cache, JSON import, synthetic data generator |
| Analytics | CAGR, cumulative return, maximum drawdown, win rate, trade-level P&L, η², Spearman ρ |
| Testing | Node test suites for the engine, sweeps, factors and navigation; Python test for the real-data builder |

## 🚀 Getting Started

Requirements: **Python 3.10+** and **Node.js 22+**.

```bash
sh start.sh            # Windows: start.bat
# or manually
pip install -r requirements.txt
python -m uvicorn backend.app:app --host 127.0.0.1 --port 8000
```

Open **http://localhost:8000** (API docs at `/docs`). A clearly labeled **synthetic** dataset loads by default.

```bash
python -m backend.realdata           # build data/market.sqlite (≈ 5 minutes)
export SEC_USER_AGENT="Your Name you@example.com"
export FMP_API_KEY="your-key"        # optional FMP connector
```

See [`GUIDE.md`](GUIDE.md) for the full engine rules, chart windows, data sources and JSON format.

## 🗂️ Project Structure

```text
Revenue-Dip-Lab
├── backend/
│   ├── app.py          FastAPI — backtest API, data jobs, FMP connector
│   ├── realdata.py     Yahoo Finance + SEC EDGAR → SQLite builder
│   └── runner.cjs      Node runner for the shared engine
├── dist/
│   ├── engine.js       backtest engine (shared by browser and API)
│   ├── sweep*.js       CAGR surface sweep + worker
│   ├── factor*.js      factor search + worker
│   └── app.js, playback.js, stock-charts.js, navigation.js, index.html, style.css
├── tests/              engine, sweep, factors, navigation, charts, real-data tests
├── GUIDE.md            detailed user and methodology guide
├── Dockerfile · start.sh · start.bat · requirements.txt
```

## 📝 Notes & Limitations

- **Synthetic ≠ evidence.** The default synthetic dataset only exercises the interface; it does not establish strategy performance.
- **Survivorship bias.** The real-data universe is today's S&P 500 constituents re-ranked weekly; companies that left the index are missing.
- **Look-ahead.** Revenue uses SEC original filing dates; the FMP path can include restatements and is not guaranteed point-in-time.
- **Not modeled:** cash dividends, taxes, FX, halts, delisting settlements, liquidity and market impact.
- Any claim of alpha needs benchmark comparison, risk adjustment and statistical validation. Research and educational use only — not investment advice.

**Next:** benchmark comparisons · threshold sensitivity · market- and sector-adjusted returns · factor exposure · risk-adjusted metrics · walk-forward validation · significance tests.

---

<div align="center"><sub>Built by <a href="https://github.com/HyeonbinJung">Hyeonbin Jung</a> · part of <a href="../">Signals to Alpha</a></sub></div>
