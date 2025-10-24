# Renewable Energy Merchant Valuation

# Hackathon Submission

## Overview

This project develops a transparent, data-driven framework to value merchant renewable energy assets
after their PPA expiration. The model integrates historical generation and market price data, forward
curves, and Monte Carlo simulations to determine **P75 hedge prices** — the fixed price level at which the
company is 75% confident it would outperform merchant exposure.

## Tools & Libraries

```
Python 3.10+
Pandas, NumPy
Matplotlib
openpyxl (Excel I/O)
```
## Files

```
File Description
```
```
HackathonDataset.xlsx
Raw historical hourly data for ERCOT,
MISO, CAISO
```
```
Forward_Curve_From_History.xlsx Monthly peak/off-peak forwards built
from historical prices
```
```
Hourly_Forward_Prices_All_Markets.xlsx Hourly forward busbar projections
```
```
Baseline_Merchant_Revenue.xlsx
Expected merchant revenues and
effective prices
P75_Hedge_Prices_With_Negative_Curtailment.xlsx Final P75 hedge prices by market
```
```
main_model.ipynb Single notebook executing entire
pipeline
```
## Environment Setup

```
# Option 1: pip
pipinstall pandasnumpy matplotlibopenpyxl
```
```
# Option 2: conda
condacreate -n hackathonpython=3.10-y
condaactivate hackathon
pipinstall pandasnumpy matplotlibopenpyxl
```
## Execution Steps (Single File)

All steps are executed in main_model.ipynb.

### 1. Data Load & Cleaning

```
Load ERCOT, MISO, CAISO data from the Excel file.
Standardize column names, extract month, hour, and determine is_peak using P/OP.
```
### 2. Forward Curve Generation

```
Calculate monthly average hub prices.
Escalate prices 2% annually for 2026–2030.
Split peak/off-peak via shape factors.
Output saved to Forward_Curve_From_History.xlsx.
```
### 3. Hourly Price Projection

```
Multiply monthly forwards by shape factors.
Add expected basis to convert hub to busbar prices.
Output saved to Hourly_Forward_Prices_All_Markets.xlsx.
```
### 4. Merchant Revenue Calculation

```
Overlay hourly prices with historical generation shape.
Compute expected energy volumes and revenues.
Output saved to Baseline_Merchant_Revenue.xlsx.
```
### 5. Monte Carlo Simulation (P75)

```
Add 20% price and 5% generation volatility.
1,000 simulation paths per market.
Negative price curtailment : if simulated price < 0 → revenue = 0.
Calculate 25th percentile of simulated prices.
Output saved to P75_Hedge_Prices_With_Negative_Curtailment.xlsx.
```
### 6. Visualization

```
Plot histograms of simulated price distributions.
Mark expected and P75 prices per market.
```
## Key Parameters

```
Parameter Description Default
```
```
escalation_rate Annual escalation for forward prices 0.
```
```
price_vol Price volatility 0.
```
```
gen_vol Generation volatility 0.
```
```
n_sim Monte Carlo simulation runs 1000
```
```
negative_price_rule Curtail revenue at negative price Enabled 
```
## Methodology Summary

```
Forward Pricing — Derived from historical hub prices, escalated and shaped hourly with peak/off-
peak splits and basis adjustments.
Risk Modeling — Applied stochastic shocks to price and volume to simulate merchant exposure
uncertainty.
Curtailment Handling — Set revenue to zero for negative price events, mimicking real offtake
conditions.
P75 Hedge Price — 25th percentile of simulated price distribution, representing a risk-adjusted
hedge level.
```
## Sample Output Table

```
Market Expected Price ($/MWh) P75 Hedge Price ($/MWh) Key Drivers
```
```
ERCOT 87.4 76.2 High volatility, negative pricing
```
```
MISO 57.2 52.1 Stable pricing
CAISO 36.5 31.7 Solar saturation & curtailment
```
## Interpretation

```
ERCOT exhibits the highest volatility and largest spread between expected and P75 prices — strong
hedging case.
MISO is relatively stable — merchant exposure may be tolerable.
CAISO’s negative pricing significantly affects P75 — curtailment risk is a key driver.
```
## Optional Enhancements

```
P50 / P90 hedge levels
Sensitivity tables (e.g., volatility ±10%)
Scenario comparison: merchant vs hedged revenues
```
## Acknowledgments

```
Data: anonymized ERCOT, MISO, CAISO generation & price data
Methodology: based on practical merchant pricing practices in US wholesale power markets
Negative price modeling included to better reflect real-world risk
```


