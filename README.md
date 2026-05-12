# tariff_impact_dashboard
Data analytics and visualization project examining how changes in U.S. import tariffs affect consumer prices across three key industries: electronics, food, and apparel.

# Inflationary Impact of U.S. Tariffs — Interactive Analytics Dashboard
### Data & Visual Analytics

https://public.tableau.com/app/profile/yu.zhou7576/viz/TariffImpactDashboard/TariffDB?publish=yes

---

## The Question

How do changes in U.S. import tariffs flow through to consumer prices - and can we build a tool that lets anyone simulate that relationship interactively?

No single public tool existed that allowed users to dynamically explore tariff pass-through effects and model inflationary scenarios across industries.

---

## What We Built

An end-to-end econometric pipeline and interactive Tableau dashboard examining tariff pass-through effects on consumer prices across three industries: **electronics, food, and apparel**, using 10 years of historical data.

---

## Data Sources

| Dataset | Source | Coverage |
|---|---|---|
| Consumer Price Index (CPI) | U.S. Bureau of Labor Statistics (BLS) | 2013–2023 |
| Producer Price Index (PPI) | BLS | 2013–2023 |
| Tariff rates | U.S. International Trade Commission (USITC) DataWeb | 2013–2023 |
| Macroeconomic indicators | Federal Reserve Economic Data (FRED) | 2013–2023 |

Tariff records were classified to industries using keyword matching against Harmonized Tariff Schedule (HTS) codes (e.g. "meat", "fruit", "vegetable" -> food industry).

---

## Methodology

### Exploratory Data Analysis
- Correlation matrix (R base library) examining relationships between CPI, PPI, and tariff data across industries
- ADF stationarity tests and ACF/PACF plots confirmed non-stationarity -> log transformations and differencing applied
- Strong tariff–price correlations provided statistical justification for ARIMAX modelling

### Modelling

Two forecasting approaches were developed and compared:

**Baseline - Holt-Winters ETS (Exponential Smoothing)**
- Captures level, trend, and seasonality
- Used to project both tariff rates and CPI as an interpretable reference

**Primary -   ARIMAX (ARIMA with Exogenous Variables)**
- Tariff rates included as an external regressor in CPI forecasting
- Industry-specific models selected via Bayesian Information Criterion (BIC)
- Trained on data through 2023, evaluated against 2024 actuals

| Industry | Final Model | MAPE (ARIMAX) | MAPE (ETS) |
|---|---|---|---|
| Apparel | ARIMA(0,1,2) | **0.50%** | 1.02% |
| Food | ARIMA(0,2,3) | — | — |
| Electronics | ARIMA(5,2,2) | — | — |
| Overall CPI | ARIMA(0,2,4) | — | — |

Models were retrained on the full dataset before deployment to the dashboard simulation engine.

---

## Dashboard

The interactive Tableau dashboard was built around four components:

1. **Industry Metrics Explorer** — tariff, CPI, and PPI trends over time per industry
2. **Tariff–PPI and Tariff–CPI Relationship Charts** — visualising pass-through dynamics
3. **Global Tariff Heat Map** — MFN tariff intensity by country
4. **CPI Simulator** — the core feature: select a month, industry, and hypothetical tariff rate → see the model's predicted CPI output instantly

All model outputs were exported as CSV and connected to Tableau as live data sources.

---

## Results

| Metric | Value |
|---|---|
| Tableau Public views | 130+ |
| Overall user satisfaction | 4.56 / 5 |
| Ease of navigation | 4.61 / 5 |
| Helpfulness of CPI Simulator | 4.56 / 5 |
| Survey responses | 18 |

---

## Tech Stack

| Category | Tools |
|---|---|
| Modelling | R (forecast library) - ARIMAX, Holt-Winters ETS |
| Statistical testing | ADF test, ACF/PACF, BIC model selection |
| Visualisation | Tableau Public |
| Data pipeline | CSV exports, R data wrangling |
| User evaluation | Microsoft Forms |

---

## Repo Structure

```
tariff_impact_dashboard/
│
├── code/
│   ├── DVAProject_ARIMAX.ipynb
│   ├── DVA_Project_ExponentialSmooting.ipynb
│
├── data/
│   ├── all cpi csvs
│   ├── all ppi csvs
│
├── dashboard/
│   └── tableau dashboard/       
│
└── README.md
```

---


# Run modelling pipeline
source("notebooks/01_data_cleaning.R")
source("notebooks/02_eda_correlation.R")
source("notebooks/03_arimax_modelling.R")
```
