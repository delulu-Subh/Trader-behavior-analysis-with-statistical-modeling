# Sentiment Regime & Trader Performance
### Evidence from 84,685 Crypto Trades · Hyperliquid Perpetuals · Behavioral Finance

> **⚠ Sample size is limited (n=32 traders). All findings are exploratory, within-sample, and non-causal. Not financial advice.**

---

## Overview

An independent quantitative study examining how Bitcoin Fear & Greed sentiment cycles correlate with per-trade profitability, directional bias, and execution consistency across active Hyperliquid perpetual futures traders.

**Primary finding:** Fear-regime trades are associated with statistically higher average realized PnL than Greed-regime trades (p < 0.001, Welch t-test). The Fear-regime average PnL of $126.41 per trade is **2.73×** greater than the Extreme Greed-regime average of $46.23.

**Live report →** *(link coming soon)*

---

## Key Metrics at a Glance

| Metric | Value |
|---|---|
| Unique Traders | 32 |
| Closed Trades Analyzed | 84,685 |
| Total Realized PnL (all traders) | $7.29M |
| Overall Win Rate | 83.5% *(survivorship-biased — see Limitations)* |
| Study Window | December 2023 – May 2025 |
| Venue | Hyperliquid (Perpetual Futures) |
| Sentiment Source | Alternative.me Fear & Greed Index |
| Analysis Completed | April 2026 |

---

## Table of Contents

- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
  - [Sentiment-Stratified Performance](#sentiment-stratified-performance)
  - [Directional Positioning](#directional-positioning)
  - [Direction-Stratified ROI](#direction-stratified-roi)
  - [Trader Segmentation](#trader-segmentation)
- [Statistical Testing](#statistical-testing)
- [Machine Learning](#machine-learning)
- [Limitations](#limitations)
- [Repository Structure](#repository-structure)
- [Tech Stack](#tech-stack)
- [Author](#author)
- [Disclaimer](#disclaimer)

---

## Dataset

**Trade data** was collected via the Hyperliquid public API, which exposes closed perpetual futures records including entry/exit price, realized PnL, position size, and direction (long/short).

Traders were selected by querying the public leaderboard for accounts with a minimum threshold of **500 closed trades** during the study window, yielding **32 qualifying addresses**. No profitability filter was applied during selection; however, the sample reflects only active accounts, introducing survivorship bias.

**Sentiment data** was sourced from the Alternative.me Fear & Greed Index — a composite daily score (0–100) aggregating volatility, market momentum, social volume, dominance, and Google Trends signals for Bitcoin. Each closed trade was assigned the daily sentiment score corresponding to its close date and classified into one of five regimes:

| Regime | Score Range | Trades in Sample |
|---|---|---|
| Extreme Fear | 0 – 24 | 9,358 |
| Fear | 25 – 44 | 26,481 |
| Neutral | 45 – 54 | 15,843 |
| Greed | 55 – 74 | 19,320 |
| Extreme Greed | 75 – 100 | 13,683 |

---

## Methodology

### Statistical Methods

- **Exploratory Data Analysis (EDA):** Group summary statistics and distribution analysis across all five sentiment regimes.
- **One-Way ANOVA:** Tests whether mean PnL differs significantly across the five regimes.
- **Welch T-Tests:** Pairwise regime comparisons with Welch's correction applied due to unequal group variances.
- **Logistic Regression:** Binary classification of trade outcome (win/loss) using only independent features — sentiment encoding, position size, and direction. ROI deliberately excluded to prevent target leakage.
- **5-Fold Stratified Cross-Validation:** Used to validate ML model generalizability.

### Tools

```
Python · pandas · scikit-learn · scipy.stats · Chart.js · Hyperliquid API · Alternative.me API
```

---

## Results

### Sentiment-Stratified Performance

| Sentiment Regime | Trades | Avg PnL (USD) | Win Rate | Avg Trade Size | PnL Std Dev |
|---|---|---|---|---|---|
| Extreme Fear | 9,358 | $95.25 | 79.96% | $5,880 | $1,669 |
| **Fear** | **26,481** | **$126.41** | **88.59%** | **$8,857** | **$1,418** |
| Neutral | 15,843 | $68.32 | 83.00% | $6,110 | $775 |
| Greed | 19,320 | $69.17 | 76.08% | $6,614 | $1,664 |
| Extreme Greed | 13,683 | $46.23 | 87.39% | $3,488 | $428 |

**Notable:** The Extreme Greed regime shows a high win rate (87.4%) paired with severely compressed outcome variance (std dev = $428), suggesting smaller, tighter-margin trades rather than genuine high-performance execution.

**Notable:** Greed yields the lowest overall win rate (76.1%) of any regime despite a broadly optimistic market backdrop — the combination is statistically robust across Welch t-tests.

### Directional Positioning

This cohort systematically shifted toward net-short positioning as sentiment rose:

| Regime | Long % | Short % |
|---|---|---|
| Extreme Fear | 66.7% | 33.3% |
| Fear | 65.2% | 34.8% |
| Neutral | 63.1% | 36.9% |
| Greed | 41.4% | 58.6% |
| Extreme Greed | 52.5% | 47.5% |

This structural pattern is consistent with contrarian orientation in this cohort, though no causal mechanism is established.

### Direction-Stratified ROI

| Regime | Long ROI (%) | Short ROI (%) |
|---|---|---|
| Extreme Fear | 1.71 | 2.72 |
| Fear | 1.90 | **6.67** |
| Neutral | 1.19 | 3.81 |
| Greed | **3.21** | 1.41 |
| Extreme Greed | 2.59 | 3.07 |

**Counterintuitive finding:** Long ROI is highest during Greed (3.21%), not Fear — price momentum on the long side was a statistically real factor during elevated sentiment periods (p < 0.0001, t = −10.68). Short-side ROI peaks during Fear at **6.67%**, the highest of any sentiment/direction combination in the dataset.

### Trader Segmentation

Top-tier traders defined as cumulative realized PnL ≥ $370K (7 of 32 traders).

| Regime | Top 20% Avg PnL | Bottom 80% Avg PnL | Top 20% Win Rate | Bottom 80% Win Rate |
|---|---|---|---|---|
| Extreme Fear | $180.35 | $51.83 | 94.0% | 72.8% |
| Fear | $190.26 | $64.63 | 93.1% | 84.3% |
| Neutral | $136.75 | $22.76 | 95.3% | 74.8% |
| Greed | $182.69 | $5.95 | 81.1% | 73.3% |
| Extreme Greed | $57.62 | $41.32 | 89.5% | 86.5% |

Top-tier traders maintained win rates above **93% across all five sentiment regimes**. The bottom 80% ranged from 73.3% to 87.4%, with most pronounced underperformance during Greed conditions — where bottom-tier average PnL collapsed to $5.95 per trade.

---

## Statistical Testing

| Test | Statistic | p-value | Result |
|---|---|---|---|
| One-Way ANOVA (PnL across all 5 regimes) | F = 11.13 | 5.1 × 10⁻⁹ | ✅ Significant |
| Welch t-test: Fear vs. Greed (Avg PnL) | t = 3.87 | 0.000111 | ✅ Fear > Greed (p < 0.001) |
| Welch t-test: Ext. Fear vs. Ext. Greed (Avg PnL) | t = 2.78 | 0.0055 | ✅ Ext. Fear > Ext. Greed (p < 0.01) |
| Welch t-test: Long ROI — Fear vs. Greed | t = −10.68 | 1.6 × 10⁻²⁶ | ⚠️ Greed Long ROI > Fear Long ROI |

*Welch correction applied throughout due to confirmed unequal group variances. Statistical significance does not imply causation.*

**Baseline context:** A naive majority-class classifier (predicting "win" for every trade) achieves ~83.5% accuracy on this dataset. Any model must exceed this threshold to demonstrate genuine predictive signal.

---

## Machine Learning

**Model:** Logistic Regression — binary classification of trade outcome (win/loss)

**Features (independent variables only):**
- `sentiment_encoding`
- `position_size`
- `direction_encoding`

> **Note on excluded models:** Random Forest and XGBoost were tested but excluded due to near-perfect scores caused by ROI-linked target leakage. Positive ROI is deterministically equivalent to a win label — including ROI as a feature constitutes data leakage, not predictive signal. All exclusions are documented in the project notebook.

| Metric | Value |
|---|---|
| Test Accuracy | 91.75% |
| Majority-Class Baseline | ~83.5% |
| Improvement over baseline | +8.25 pp |
| ROC-AUC | 98.32% |
| CV AUC (5-fold stratified) | 97.96% |

**Feature importance (normalized logistic regression coefficient magnitudes):**

```
sentiment_encoding   ████████████████████████████████  52%
position_size        ████████████████████              31%
direction_encoding   ██████████                        17%
```

Sentiment encoding is the dominant predictor, followed by position size and trade direction.

---

## Limitations

These constraints are declared transparently and must be considered before interpreting any result.

| Limitation | Description |
|---|---|
| **Small trader sample (n = 32)** | Statistical significance is established within-sample only. Findings are exploratory, not confirmatory, and cannot be generalized to the broader population. |
| **Survivorship bias** | All 32 traders were actively trading at data collection time. Accounts that blew up or became inactive are entirely absent, inflating win rates and average PnL across all regimes. |
| **No causal inference** | Association is not causation. Confounding variables — market volatility, BTC price trend, trader learning effects — are not controlled for. |
| **No per-trade leverage data** | Leverage is unavailable at the trade level. Risk-adjusted return analysis is impossible; any Sharpe-like proxy is directionally indicative only. |
| **Single venue (Hyperliquid)** | Funding rate structures, liquidity depth, and order flow differ across venues. Findings may not replicate on Binance, Bybit, dYdX, or others. |
| **Single asset sentiment mapping** | The Fear & Greed Index is Bitcoin-specific. Cross-asset validity of this sentiment mapping is untested. |

> This is observational, cross-sectional analysis. No experiment was conducted and no causal mechanism is established. Findings should not be interpreted as predictive rules or investment guidance.

---

## Repository Structure

```
├── data/
│   ├── raw/                   # Raw trade records from Hyperliquid API
│   ├── processed/             # Merged trade + sentiment dataset
│   └── fear_greed_index.csv   # Daily Alternative.me F&G scores
├── notebooks/
│   ├── 01_data_collection.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_statistical_tests.ipynb
│   └── 04_ml_model.ipynb
├── report/
│   └── index.html             # Interactive web report
├── requirements.txt
└── README.md
```

---

## Tech Stack

| Category | Tools |
|---|---|
| Data Collection | Hyperliquid Public API, Alternative.me API |
| Data Processing | Python, pandas |
| Statistical Analysis | scipy.stats (ANOVA, Welch t-test) |
| Machine Learning | scikit-learn (Logistic Regression, StratifiedKFold) |
| Visualization | Chart.js |
| Report | HTML, CSS, JavaScript |

---

## Author

**Subham Dey** — Data Analyst & Independent Researcher · Behavioral Finance · Crypto Market Microstructure

This project originated from a question that existing research hadn't answered well: do sentiment cycles measurably shape trade-level outcomes among active perpetual futures traders, not just prices? The Hyperliquid API made detailed trade data accessible for the first time at scale, enabling a systematic analysis that would have been difficult on centralized venues. The goal was to produce honest, reproducible quantitative research — not a signal product or trading system.

[![GitHub](https://img.shields.io/badge/GitHub-delulu--Subh-blue?logo=github)](https://github.com/delulu-Subh)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-subham--dey--cs-blue?logo=linkedin)](https://www.linkedin.com/in/subham-dey-cs/)

---

## Disclaimer

> All findings are observational, within-sample, and subject to survivorship bias. No causal claims are made. This is not financial advice. Past patterns in a limited sample do not predict future market behavior. Trading decisions based on this research carry substantial risk and are not recommended.

© 2026 Subham Dey · Independent Research · Not Financial Advice
