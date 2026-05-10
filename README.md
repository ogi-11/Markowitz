# Markowitz Portfolio Optimisation

A Python implementation of Modern Portfolio Theory (MPT) using Monte Carlo simulation to construct the efficient frontier across a 16-asset equity portfolio.

## Overview

This project applies Markowitz mean-variance optimisation to identify two optimal portfolios from a universe of 16 stocks and ETFs:

- **Minimum variance portfolio** — lowest achievable volatility for the given asset universe
- **Maximum Sharpe ratio portfolio** — optimal risk-adjusted return

The efficient frontier is constructed by simulating 1,000,000 random portfolio weight combinations and plotting the resulting return/volatility space.

## Methodology

### Data
- 16 assets spanning US tech, healthcare, financials, consumer staples, REITs, and broad market ETFs (GOOGL, AMZN, NVDA, AAPL, MSFT, JPM, BLK, V, PG, AZN, PFE, PLD, XWD.TO, NTR, VTRS, QQQ)
- 7 years of daily price data sourced via `yfinance` (March 2018 – March 2025)
- Train/test split: last 252 trading days held out as out-of-sample test period

### Return estimation
- Arithmetic mean annual returns (daily mean × 250)
- Geometric (CAGR) returns computed over the 6-year training window for comparison
- Annual covariance matrix derived from daily returns (scaled by 250)

### Optimisation
- Monte Carlo simulation: 1,000,000 portfolios with randomly sampled weights (long-only, fully invested)
- Each portfolio evaluated on: expected annual return, annualised volatility (portfolio standard deviation), Sharpe ratio (assuming 0% risk-free rate)
- Efficient frontier visualised as a volatility/return scatter coloured by Sharpe ratio

### Out-of-sample backtest
Both optimal portfolios were evaluated on the held-out test year (March 2024 – March 2025), starting from a $1,000 notional investment.

## Results

| Portfolio | Expected Return | Volatility | Sharpe Ratio |
|---|---|---|---|
| Minimum variance | 13.6% | 16.8% | 0.81 |
| Maximum Sharpe | 25.2% | 22.1% | 1.14 |

**Out-of-sample performance ($1,000 invested March 2024):**

| Portfolio | Ending value |
|---|---|
| Minimum variance | $1,149.69 |
| Maximum Sharpe | $1,211.47 |

The maximum Sharpe portfolio is heavily weighted toward NVDA (~20%) and AAPL (~13%), reflecting the strong momentum in US large-cap tech over the training period. The minimum variance portfolio diversifies more broadly, with significant weights in AZN, PG, and XWD.TO (global equity ETF).

## Tech stack

- `yfinance` — market data
- `pandas`, `numpy` — data manipulation and linear algebra
- `scipy` — statistical utilities
- `matplotlib` — efficient frontier visualisation

## Limitations & extensions

- Risk-free rate assumed to be 0% for Sharpe calculation; a more accurate implementation would use the prevailing T-bill rate
- Long-only constraint only; no shorting or leverage
- Return estimates are backward-looking; regime changes are not accounted for
- Future work: add Black-Litterman views, shrinkage estimators for the covariance matrix (Ledoit-Wolf), and rolling-window backtests
