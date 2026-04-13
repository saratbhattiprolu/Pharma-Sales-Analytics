# 💊 Pharma Sales Forecasting

> Turning historical pharmaceutical sales data into actionable forward-looking demand signals using ARIMA and Facebook Prophet.

## Business Problem

Pharmaceutical supply chains need accurate demand signals at two horizons:
- **12 weeks ahead** for weekly replenishment and S&OP planning
- **52 weeks ahead** for annual procurement and supplier contracting

This project builds those signals for 8 drug categories across the ATC classification system.

## Approach

| Stage | What was done |
|---|---|
| Exploratory Analysis | Stationarity tests, seasonality decomposition, noise profiling |
| Model Evaluation | ARIMA, Auto-ARIMA, Prophet, Vanilla/Stacked/Bidirectional LSTM |
| Forecasting | Best model per drug trained on full data → real future forecasts |
| Output | CSVs with point forecasts + 95% confidence/uncertainty intervals |

## Drug Portfolio

| Code | Drug | Tier | Noise |
|---|---|---|---|
| M01AB | Diclofenac | Tier 1 — Highly Predictable | 14.8% |
| M01AE | Ibuprofen | Tier 1 — Highly Predictable | 15.4% |
| N02BA | Aspirin | Tier 1 — Structural Decline Watch | 14.5% |
| N02BE | Paracetamol | Tier 1 — Highly Predictable | 13.6% |
| N05B | Anxiolytics | Tier 2 — Moderate Noise | 20.9% |
| N05C | Sedatives | Tier 3 — High Volatility | 52.6% |
| R03 | Respiratory Drugs | Tier 2 — Strongly Seasonal | 29.3% |
| R06 | Antihistamines | Tier 2 — Strongly Seasonal | 21.7% |

## Outputs

- `outputs/forecasts/arima_12w_forecasts.csv` — 12-week forward forecasts (all drugs)
- `outputs/forecasts/prophet_52w_forecasts.csv` — 52-week forward forecasts (all drugs)
- `outputs/forecasts/forecast_summary.csv` — Portfolio-level summary table
- `outputs/charts/` — Publication-quality forecast visualisations

## Setup

```bash
pip install pandas numpy matplotlib seaborn statsmodels prophet
jupyter notebook pharma_sales_forecasting.ipynb
```

## Data

[Pharmaceutical Drug Spending Dataset](https://www.kaggle.com/datasets/milanzdravkovic/pharma-sales-data) — Kaggle.  
Weekly sales data across 8 ATC drug categories.

## Skills Demonstrated

`Time Series Forecasting` · `ARIMA` · `Facebook Prophet` · `Uncertainty Quantification` · `Supply Chain Analytics` · `Python` · `statsmodels`