# Executive Summary
## Pharmaceutical Sales Analysis and Demand Forecasting
**Prepared by:** Data Science and Analytics  
**Classification:** Internal — Strategic Use  
**Date:** Q2 2025

---

## Purpose

This document summarises the findings from a multi-phase data science project covering historical demand analysis and forward-looking sales forecasting across the company's portfolio of eight pharmaceutical drug categories. The analysis was conducted on 6+ years of weekly pharmacy-level dispensing data and is intended to directly inform supply chain strategy, procurement planning, and commercial portfolio decisions.

This is not a modelling report. Every finding in this document is expressed in terms of the business decision it affects.

---

## The Core Finding

The company is managing eight fundamentally different demand profiles under one planning process. That mismatch is almost certainly creating unnecessary stockouts on seasonal products, excess working capital on stable products, and reactive rather than planned procurement across the portfolio.

The data makes it possible to run a differentiated planning process. This document explains what that looks like.

---

## Portfolio Segmentation

Analysis of demand noise, trend stability, and seasonal structure across all eight drugs produces a clear three-tier segmentation:

### Tier 1 — Highly Predictable (13–16% demand noise)

**Drugs:** Paracetamol (N02BE), Diclofenac (M01AB), Ibuprofen (M01AE), Aspirin (N02BA)

These four products have the most stable demand profiles in the portfolio. Week-to-week sales are driven by consistent, recurring patient need rather than episodic or physician-driven events. Statistical models can explain 84–87% of all demand movement in these categories.

**Supply chain implication:** These drugs are candidates for automated, model-driven replenishment with tight inventory targets. The analytical work done in this project is sufficient to deploy a production-ready weekly replenishment signal immediately. Running lean on these categories is low-risk.

**Exception — Aspirin:** Aspirin passes the noise threshold but fails the stationarity test. It is the only product in the portfolio where demand is drifting downward over time in a statistically significant way. This is addressed separately below.

---

### Tier 2 — Moderate Noise, Seasonal Character (21–29% demand noise)

**Drugs:** Antihistamines (R06, 21.7%), Anxiolytics (N05B, 20.9%), Respiratory Drugs (R03, 29.3%)

These drugs have more variable demand, driven by a combination of underlying seasonal cycles and patient-level variability. Importantly, the seasonal structure in R03 and R06 is strong and repeating — making the seasonal component plannable even if week-to-week noise is higher than Tier 1.

**R03 — Respiratory:** Peak demand arrives November through February, driven by respiratory illness season. Demand in peak months is meaningfully higher than the off-season baseline. Procurement decisions for this category need to be made 6–8 weeks in advance of the peak — not in response to it.

**R06 — Antihistamines:** The allergy season cycle drives clear demand waves from March through July. The off-season risk is overstock and margin erosion from unsold inventory. The in-season risk is stockouts during peak allergy months.

**Supply chain implication:** A flat replenishment model will systematically fail these categories. The 52-week Prophet forecast produced in this project explicitly models the annual demand cycle and provides a procurement-ready planning signal for both drugs.

---

### Tier 3 — High Volatility (52.6% demand noise)

**Drug:** Sedatives (N05C)

Sedatives are categorically different from the rest of the portfolio. Over half of all observed demand movement cannot be explained by any historical trend, seasonal pattern, or statistical model. This is not a data quality issue and it is not a modelling failure. It reflects the nature of sedative prescribing: decisions are made at the individual physician and patient level, are sensitive to clinical guidance and regulatory signals, and are not predictable from aggregate sales history.

**Supply chain implication:** Do not manage sedatives the same way you manage paracetamol. The forecasting models produced for this drug show wide confidence intervals by design — that width is the correct and honest signal. Safety stock should be set to the upper bound of the confidence interval, not the mean forecast. The S&OP team should maintain a standing agenda item for external signal monitoring: regulatory changes, new prescribing guidelines, and competitor shortages are the real demand drivers here, and none of them will appear in the data before they affect sales.

---

## The Aspirin Situation

Aspirin deserves a separate conversation. It is the only product in the portfolio where demand is not statistically stable — the ADF test confirms a structural downward drift that cannot be explained by noise or seasonality.

The direction of this trend is consistent with well-documented market-level dynamics: patients and physicians are migrating toward paracetamol and ibuprofen as first-line analgesics, driven by clinical guidance on aspirin's gastrointestinal side effects and the availability of equally effective alternatives.

Three questions the commercial team should be able to answer before the next annual planning cycle is locked:

1. Is this trend present in our specific customer base and geographies, or only at the market level?
2. Does our current shelf space and promotional investment reflect a declining category or a stable one?
3. What is the minimum viable procurement volume for aspirin given declining demand, and at what point does it become economically unattractive to stock?

These are not data science questions. They are commercial decisions that the data is flagging as overdue.

---

## Forecasting Outputs

The project produced two operational forecasting signals:

### 12-Week ARIMA Forecasts (Short-Term Replenishment)

ARIMA models, trained on the complete historical dataset, generate a 12-week forward demand signal for all eight drugs with 95% confidence intervals. These are intended for weekly S&OP input and replenishment decisions.

The models outperform naive planning baselines across 7 of 8 drugs. For Tier 1 analgesics and anti-inflammatories, the confidence intervals are tight enough to support automated replenishment decisions. For Tier 2 and Tier 3 drugs, the wider intervals communicate the appropriate level of uncertainty to planners.

### 52-Week Prophet Forecasts (Annual Procurement Planning)

Prophet models, using logistic growth with a physical floor of zero, generate a full annual planning horizon for all eight drugs. The seasonal demand cycle is explicitly modelled for R03, R06, and N02BE, meaning the forecast captures demand peaks rather than averaging them away.

For seasonal drugs specifically, these forecasts represent a material improvement over any flat or rolling-average baseline currently in use. The peak-to-trough demand difference visible in the Prophet output is the procurement swing that needs to be pre-positioned before each season begins.

All forecast outputs are available as structured CSV files compatible with any planning tool or Excel-based process.

---

## Three Decisions for Leadership

The analysis and forecasting work is complete. The value is unlocked by making three decisions:

**Decision 1: Separate the portfolio into distinct supply chain operating models.**
This is a COO and supply chain leadership decision. Tier 1 drugs should move to lean, model-driven replenishment. Tier 2 seasonal drugs should move to forward-positioned seasonal procurement. Tier 3 (sedatives) should be managed under an explicit buffer policy with active external signal monitoring. Running all eight drugs through the same planning process is the source of unnecessary cost and service risk.

**Decision 2: Commission a commercial review on aspirin before the next annual plan.**
This is a CSO decision. The data flags a structural trend — the business decision of what to do about it belongs to commercial and medical affairs. The review should be scoped to validate the trend at the customer and geography level, and produce a recommendation on shelf space, promotional investment, and procurement volumes.

**Decision 3: Deploy the 12-week ARIMA forecasts into the S&OP process immediately.**
This is an operational decision. The models are ready, the outputs are structured, and deployment requires only connecting the CSV output to the existing planning process. This decision has the shortest time-to-value of the three.

---

## What This Does Not Cover

This analysis is based on internal dispensing data. It does not incorporate:

- Competitor activity or market share dynamics
- Prescriber-level demand signals (available via IQVIA Xponent or similar)
- Pipeline products that may shift demand within categories
- Pricing or promotional effects on volume

Incorporating IQVIA market-level data would materially improve forecast accuracy, particularly for seasonal and high-volatility categories. If IQVIA licenses are already in place, integrating that data into the existing model framework is a well-scoped next step.

---

## Appendix: Technical Methodology Summary

| Stage | Methods Used |
|---|---|
| Trend and seasonality exploration | Rolling means (30-day, 365-day), seasonal decomposition (additive, period=52) |
| Stationarity testing | Augmented Dickey-Fuller (ADF), KPSS |
| Predictability quantification | Approximate Entropy, residual noise as % of observed sales |
| Model evaluation | Naive, Seasonal Naive, ARIMA, Auto-ARIMA, Prophet, Vanilla/Stacked/Bidirectional LSTM |
| Short-term forecasting | ARIMA (per-drug orders from evaluation), 12-week horizon, 95% CI |
| Long-term forecasting | Prophet with logistic growth, floor=0, per-drug changepoint priors, 52-week horizon |
| Seasonal drugs | Prophet custom yearly seasonality (R03, R06, N02BE) |
| Validation approach | Held-out last 50 weeks for model evaluation; full-data training for production forecasts |

---

*Data source: Internal weekly dispensing records, supplemented by Kaggle pharmaceutical sales dataset for methodology development.*  
*For questions on methodology or outputs, contact the Data Science and Analytics team.*
