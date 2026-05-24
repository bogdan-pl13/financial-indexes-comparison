# Index Types Comparison

A Python class that constructs and compares three equity index methodologies — Price-Weighted (PWI), Equal-Weighted (EWI), and Value-Weighted (VWI) — against individual stock performance, with annualised risk/return analytics and a scatter plot output.

---

## What It Does

Downloads daily Close prices via `yfinance` for a user-defined ticker list and builds three synthetic indices from scratch:

| Index | Methodology | Real-World Analogue |
|-------|-------------|---------------------|
| PWI   | Sum of prices, normalised to 100 | Dow Jones Industrial Average |
| EWI   | Equal daily return contribution  | Russell 2000 (equal-weight variant) |
| VWI   | Market-cap weighted daily returns | S&P 500 |

**Outputs:**
- Annualised return, volatility (σ), and Sharpe ratio — indices only
- Risk/Return scatter plot saved as `risk_return.png`
- Error/warning summary for any data quality issues caught at runtime

---

## Output

![Risk/Return Scatter](risk_return.png)

```
=== Index Risk / Return Summary ===
      mean   std  sharpe
PWI   0.09  0.21    0.43
EWI   0.07  0.19    0.37
VWI   0.11  0.20    0.55
```

---

## Usage

```python
from stock_valuator import StockValuator

tickers = ['AAPL', 'MSFT', 'AMZN', 'BA', 'DIS', 'IBM', 'KO']
sv = StockValuator(tickers, start='2020-01-01')
sv.main()
```

Custom date range:

```python
sv = StockValuator(tickers, start='2018-01-01', end='2023-12-31')
sv.main()
```

Run directly:

```bash
python stock_valuator.py
```

---

## Requirements

```
pandas
numpy
matplotlib
yfinance
```

```bash
pip install pandas numpy matplotlib yfinance
```

---

## Known Limitations

**1. Shares outstanding assumed constant**
VWI weights are computed using the *current* shares outstanding figure fetched from `yfinance` at runtime and applied uniformly across the entire historical window. In reality, float changes continuously due to buybacks, secondary offerings, and splits. A production-grade VWI requires a time-series of historical shares outstanding — typically sourced from Compustat, Bloomberg, or a paid data provider such as Financial Modeling Prep.

**2. Hardcoded default tickers**
The default list (`AAPL, MSFT, AMZN, BA, DIS, IBM, KO`) is illustrative and not representative of any real index. It carries **survivorship bias** — every company in the list survived the full period. A rigorous study would include delistings and bankruptcies.

**3. Unadjusted close prices**
Prices are not adjusted for dividends or stock splits. This understates total returns for dividend-paying stocks (KO, IBM, DIS) and can distort pre/post-split price history. Adjusted close should be used for total return index construction.

**4. Continuous EWI rebalancing**
EWI is effectively rebalanced daily (mean of daily returns). Real equal-weight indices rebalance on a fixed schedule (typically quarterly), incurring transaction costs not modelled here.

**5. Risk-free rate assumed zero**
Sharpe ratio uses rf = 0. For a more accurate measure, subtract the annualised 3-month T-bill rate from the numerator.

---

## Improvements Roadmap

- [ ] Historical shares outstanding via FMP or Compustat for time-accurate VWI weights
- [ ] Switch to adjusted close prices for total return calculation
- [ ] Configurable rebalancing frequency for EWI (monthly, quarterly)
- [ ] Configurable risk-free rate for Sharpe ratio
- [ ] Max drawdown and recovery period metrics
- [ ] Correlation heatmap across all series
- [ ] Excel export via `openpyxl` with formatted summary sheet
- [ ] Unit tests for index construction and annualisation logic

---

## Finance Concepts Demonstrated

- Index construction methodologies: price-weighted, equal-weighted, value-weighted
- Market capitalisation weighting with **lagged weights** to avoid look-ahead bias
- Annualisation of daily return (×252) and volatility (×√252)
- Risk/Return scatter plot: visual analogue of the mean-variance frontier

---

## Author

Bogdan Plokhotnichenko
[LinkedIn](https://linkedin.com/in/bogdan-plokhotnichenko) · [GitHub](https://github.com/bogdan-pl13)
