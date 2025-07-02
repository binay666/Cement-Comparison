# Dynamic Interactions in Financial Markets

## Overview
This projects conducts a detailed financial analysis of three major Indian cement companies—Shree Cement Ltd (`SHREECEM.NS`), UltraTech Cement Ltd (`ULTRACEMCO.NS`), and Ambuja Cements Ltd (`AMBUJACEM.NS`)—from January 1, 2020, to December 30, 2024. It uses advanced econometric techniques, including GARCH models, Vector Autoregression (VAR), Johansen cointegration tests, and risk metrics, with data from Yahoo Finance. The analysis examines price trends, volatility, inter-stock relationships, and risk profiles.

### Objectives
- **Price Trends**: Analyze stock price movements.
- **Volatility**: Model risk using GARCH.
- **Inter-Stock Relationships**: Investigate dependencies via VAR and cointegration.
- **Risk Metrics**: Calculate annualized returns, volatility, and Sharpe ratios.

## Project Structure
The notebook is divided into sections with code and markdown cells. Below is a detailed breakdown of key sections, actions, outputs, and interpretations.

---

## Section 1: Data Collection and Preparation

### Code Cell 1: Import Libraries
**Purpose**: Sets up the Python environment.

**Libraries**:
- `yfinance`: Fetches stock data.
- `pandas`: Manages DataFrames.
- `numpy`: Numerical computations.
- `matplotlib.pyplot`: Visualizations.
- `statsmodels.tsa.api.VAR`: VAR models.
- `arch.arch_model`: GARCH models.
- `statsmodels.tsa.vector_ar.vecm.coint_johansen`: Cointegration tests.
- `seaborn`: Enhances plots.
- `warnings`: Suppresses warnings.

**Output**: None (sets `seaborn-v0_8` style with `husl` palette).

**Explanation**: Libraries enable data fetching, modeling, and visualization. Styling ensures professional plots.

---

### Code Cell 2: Download Stock Data
**Purpose**: Retrieves daily closing prices for the three stocks.

**Actions**:
1. Defines tickers: `['SHREECEM.NS', 'ULTRACEMCO.NS', 'AMBUJACEM.NS']` and names: `['Shree Cement', 'UltraTech Cement', 'Ambuja Cement']`.
2. Uses `yf.download` for closing prices, drops missing values.
3. Prints trading days, date range, and descriptive statistics.

**Output**:
```
 Downloading stock data...
YF.download() has changed argument auto_adjust default to True
 Successfully downloaded data for 1237 trading days
 Date range: 2020-01-01 to 2024-12-30

 Stock Price Summary:
Ticker  AMBUJACEM.NS  SHREECEM.NS  ULTRACEMCO.NS
count        1237.00      1237.00        1237.00
mean          393.02     24407.68        7179.11
std           145.05      2810.42        2282.04
min           125.00     15537.85        2945.90
25%           283.07     22671.52        5852.44
50%           381.42     24624.12        7041.03
75%           513.20     26415.12        8276.17
max           695.00     31254.03       12083.90
```

**Interpretation**:
- **Data**: 1237 trading days (~5 years).
- **Prices**:
  - **Ambuja**: Mean ₹393, std ₹145, range ₹125–₹695.
  - **Shree**: Mean ₹24,408, std ₹2,810, range ₹15,538–₹31,254.
  - **UltraTech**: Mean ₹7,179, std ₹2,282, range ₹2,946–₹12,084.
- **Insights**: Shree has the highest price/volatility, Ambuja the lowest.
- **Error Handling**: `try-except` ensures robust data retrieval.

---

## Section 2: Returns Calculation and Basic Analysis

### Code Cell 3: Calculate Returns
**Purpose**: Computes simple and log returns.

**Actions**:
1. Simple returns: `(P_t - P_{t-1}) / P_{t-1}` using `data.pct_change().dropna()`.
2. Log returns: `ln(P_t / P_{t-1})` using `np.log(data / data.shift(1)).dropna()`.
3. Prints simple returns statistics.

**Output**:
```
 Calculating returns...
 Returns Summary:
Ticker  AMBUJACEM.NS  SHREECEM.NS  ULTRACEMCO.NS
count      1236.0000    1236.0000      1236.0000
mean          0.0012       0.0004         0.0010
std           0.0225       0.0184         0.0174
min          -0.1733      -0.1029        -0.1452
25%          -0.0094      -0.0096        -0.0083
50%           0.0017       0.0007         0.0008
75%           0.0121       0.0101         0.0099
max           0.0933       0.0777         0.1288
```

**Interpretation**:
- **Simple Returns**:
  - **Ambuja**: 0.12% mean, 2.25% std (highest).
  - **Shree**: 0.04% mean, 1.84% std (lowest return).
  - **UltraTech**: 0.10% mean, 1.74% std (lowest volatility).
- **Range**: Largest loss (Ambuja: -17.33%), gain (UltraTech: 12.88%).
- **Log Returns**: Used for GARCH (not printed).
- **Insights**: Ambuja is high-return/high-risk, Shree underperforms.

---

## Section 3: Comprehensive Visualization Dashboard

### Code Cell 4: Multi-Panel Dashboard
**Purpose**: Visualizes price trends, returns, volatility, and correlations.

**Actions**:
1. Creates subplots for:
   - Normalized price trends.
   - Daily returns.
   - Rolling volatility (e.g., 21-day).
   - Correlation heatmap.
2. Uses `matplotlib` and `seaborn`.

**Output**:
```
 Creating price trend visualization...
```

![1](images/1.png)


**Interpretation**:
- **Price Trends**: Normalized plots show relative changes.
- **Returns**: Highlights volatility spikes.
- **Volatility**: Shows risk evolution.
- **Correlation**: Visualizes co-movement (values near 1 indicate strong correlation).
- **Insights**: Comprehensive view of stock behavior.
---

## Section 4: Bollinger Bands

![2](images/2.png)

---

## Section 5: GARCH (1,1) Modeling
![3](images/3.png)
![4](images/4.png)
![5](images/5.png)

---

## Section 6: VAR Analysis

### Code Cell 9: Build VAR Model
**Purpose**: Models interdependencies between stock returns.

**Actions**:
1. Uses `returns.dropna()`.
2. Selects lag length with `model.select_order(maxlags=10)`.
3. Fits VAR model using AIC-selected lag.
4. Prints model summary and plots IRFs.

**Output**:
```
 Building VAR model...
 VAR Lag Selection Results:
   • AIC: 2 lags
   • BIC: 1 lags
   • FPE: 2 lags
   • HQIC: 1 lags

 VAR(2) model fitted successfully
 Model Summary:
   • Number of equations: 3
   • Number of observations: 1234
   • Log-Likelihood: 11328.67
   • AIC: -18.34

 Computing Impulse Response Functions...

```
![6](images/6.png)


**Interpretation**:
- **VAR**: Models returns based on past returns (3 equations).
- **Lags**: AIC/FPE select 2 lags.
- **Fit**: 1234 observations, log-likelihood 11328.67, AIC -18.34.
- **IRFs**: Show shock propagation over 10 days.
- **Insights**: Captures short-term interdependencies.

---

## Section 7: Cointegration Analysis

### Code Cell 10: Johansen Cointegration Test
**Purpose**: Tests for long-run equilibrium relationships.

**Actions**:
1. Runs `coint_johansen` on prices (`det_order=0`, `k_ar_diff=2`).
2. Compares trace statistics to 5% critical values.
3. Prints results and interpretation.

**Output**:
```
 Performing Johansen Cointegration Test...

 Johansen Cointegration Test Results:
Null Hypothesis      Trace Stat   5% Critical  Reject? 
-------------------------------------------------------
r <= 0                 18.36        29.80        No      
r <= 1                 4.66         15.49        No      
r <= 2                 0.09         3.84         No      

 Interpretation:
   • Number of cointegrating relationships: 0
   • No cointegration found - stocks move independently in long run
```

**Interpretation**:
- **Cointegration**: Tests for long-term price relationships.
- **Results**: No cointegration (trace < critical values).
- **Insights**: Stocks move independently, no mean-reverting opportunities.

---

## Section 8: Summary and Risk Metrics

### Code Cell 11: Final Summary
**Purpose**: Summarizes risk metrics and correlations.

**Actions**:
1. Recalculates returns.
2. Computes annualized metrics:
   - Return: Mean × 252.
   - Volatility: Std × √252.
   - Sharpe: Return ÷ Volatility.
3. Prints metrics and correlation summary.

**Output**:
```
 FINAL SUMMARY
==================================================

 Risk Metrics (Annualized):
               Annual Return  Annual Volatility  Sharpe Ratio
AMBUJACEM.NS          0.2947             0.3575        0.8244
SHREECEM.NS           0.0949             0.2918        0.3254
ULTRACEMCO.NS         0.2516             0.2767        0.9093

 Correlation Summary:
   • Highest correlation: 0.6629
   • Lowest correlation: 0.5095
   • Average correlation: 0.5699

 Analysis completed successfully!
 All visualizations have been generated and displayed.
```

**Interpretation**:
- **Metrics**:
  - **Ambuja**: 29.47% return, 35.75% volatility, Sharpe 0.8244.
  - **Shree**: 9.49% return, 29.18% volatility, Sharpe 0.3254.
  - **UltraTech**: 25.16% return, 27.67% volatility, Sharpe 0.9093.
- **Correlations**: Moderate (0.5095–0.6629, avg 0.5699).
- **Insights**: UltraTech is best risk-adjusted, Ambuja high-risk/high-return, Shree underperforms.

---

## Further Extensions
- **Risk Management**: Value at Risk, Expected Shortfall.
- **Advanced Models**: Multivariate GARCH, regime-switching.
- **Trading Strategies**: Explore if cointegration exists.
- **Economic Indicators**: Add macroeconomic factors.

---

## Overall Insights
- **Performance**: UltraTech excels, Ambuja high-risk/high-return, Shree underperforms.
- **Volatility**: GARCH (implied) would show clustering.
- **Interdependencies**: VAR shows short-term effects, no long-run cointegration.
- **Sector Dynamics**: Moderate correlations suggest diversification.

---
