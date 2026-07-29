# Time Series Analysis: RUB/USD Exchange Rate vs. S&P 500

A time-series econometrics mini-project studying two daily financial series — the RUB/USD exchange rate and the S&P 500 index — over 2010-01-04 to 2026-05-20.

## Contents

- `analysis.ipynb` — the full analysis notebook (stationarity tests, ARMA/GARCH modeling, rolling forecasts with Diebold-Mariano comparison, structural break search, Granger causality, and cointegration analysis via Johansen test and DOLS).
- `requirements.txt` — Python packages needed to run the notebook.

## Data

The notebook expects two CSV files in the working directory:
- `USDRUB.csv` — columns `Date`, `Price` (RUB/USD daily price)
- `S&P500.csv` — columns `Date`, `Price` (S&P 500 daily price)

An optional `yfinance` cell can pull the S&P 500 series directly from Yahoo Finance (`^GSPC`) as a reproducibility check.

## Methods used

| Topic | Techniques |
|---|---|
| Stationarity | ACF/PACF inspection, Augmented Dickey-Fuller (ADF), Phillips-Perron |
| Linear dependence | Ljung-Box test, Box-Jenkins ARMA order search (AIC/BIC) |
| Volatility | ARCH-LM test, GARCH(p,q) order search |
| Forecast comparison | Rolling one-step-ahead forecasts, Diebold-Mariano test |
| Structural breaks | HAC (Newey-West) robust break-date search in mean and variance |
| Cross-market linkages | Granger causality, VAR lag selection, Johansen cointegration test, Dynamic OLS (DOLS) |

## Key findings

- Both series' **log prices** behave as unit-root processes; their **log returns** are stationary.
- Log returns retain limited linear structure (ARMA(2,2) for FX, ARMA(4,4) for equities) and strong, persistent conditional heteroskedasticity (GARCH(1,2) for FX, GARCH(3,1) for equities).
- Neither AR(5) nor ARMA(3,3) beats the other out-of-sample for either series (Diebold-Mariano test is insignificant in both cases).
- RUB/USD shows several HAC-robust structural breaks in *variance*, all before 2015 (consistent with the 2014-2015 ruble crisis); S&P 500 shows none. Neither series shows a break in *mean*.
- Weak, lag-specific Granger causality runs from FX to equities (lags 3-4 only); none in the reverse direction.
- No evidence of cointegration between RUB/USD and S&P 500 log prices (Johansen test fails to reject "no cointegration"; the seemingly significant DOLS fit is best explained as a spurious regression).

## How to run

```bash
pip install -r requirements.txt
jupyter notebook analysis.ipynb
```

## License

For educational/research use.
