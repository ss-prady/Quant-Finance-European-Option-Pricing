## Overview

This repository contains a comprehensive analysis comparing two foundational models for pricing European call and put options:

1. **Binomial Tree Model** (discrete‐time, multi‐step framework)
2. **Black‑Scholes Model** (continuous‐time, closed‐form solution)

Using historical market data for major ETFs (SPY, QQQ, DIA), the project:

* Implements both pricing models from first principles
* Fetches adjusted closing prices and dividend yields via Yahoo Finance
* Calibrates model inputs (volatility, risk‑free rate, dividend yield)
* Quantifies and visualizes pricing errors across maturities
* Interprets model performance in light of market characteristics
  
**Citation:** Hull, J. (2009). Options, futures and other derivatives. Pearson Education.
---

## Assumptions

1. **No Arbitrage**: Markets do not allow riskless profit opportunities.
2. **Risk‑Neutral Valuation**: All investors are indifferent to risk; expected returns equal the risk‑free rate.

| Assumption         | Binomial Tree Model                      | Black‑Scholes Model                        |
| ------------------ | ---------------------------------------- | ------------------------------------------ |
| Time Framework     | Discrete steps (N intervals of Δt)       | Continuous time (differential formulation) |
| Price Dynamics     | Up/down factor per step                  | Continuous lognormal diffusion             |
| Volatility         | Constant per step; converges to σ as N→∞ | Constant instantaneous volatility          |
| Interest Rate      | Discrete per‐period rate                 | Continuously compounded rate               |
| Dividend Treatment | Per‐step dividend yield q                | Continuous dividend yield δ                |

---

## Features

* **Data Acquisition**:

  * Historical adjusted close prices via `fetch_data()`
  * Dividend yields via `get_dividend_yield()`
  * Risk‑free rate via `get_risk_free_rate()`

* **Model Implementations**:

  * `binomial_price()` for European calls/puts (vectorized N‑step tree)
  * `bs_price()` closed‑form formulas and Greeks

* **Error Analysis**:

  * Absolute pricing errors across maturities (0.1–3 years)
  * Comparative plots for SPY, QQQ, DIA
  * Quantitative tables summarizing error growth

* **Visualization**:

  * Matplotlib plots of model prices vs. market quotes
  * Error curves illustrating model divergence


## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/ss-prady/Quant-Finance-European-Option-Pricing.git
   ```
2. **Create a virtual environment (optional but recommended)**

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

---

## Usage

1. **Launch the Jupyter notebook**

   ```bash
   jupyter notebook notebook.ipynb
   ```

2. **Execute cells sequentially** to:

   * Fetch data for SPY, QQQ, DIA over a specified date range
   * Compute and compare option prices under both models
   * Generate error analyses and plots

---

## Methodology

1. **Parameter Estimation**

   * **Volatility (σ)**: Annualized standard deviation of log returns
   * **Dividend Yield (q)**: Trailing‐twelve‐month dividends / current price
   * **Risk‑Free Rate (r)**: Current 3‑month Treasury yield

2. **Model Computation**

   * **Binomial**: Forward induction to terminal payoffs, backward induction for present value
   * **Black‑Scholes**: Standard d₁, d₂ formulas for European options

3. **Error Metrics**

   * Absolute difference |ModelPrice – MarketPrice|
   * Aggregated statistics (mean, max) over maturity grid

---

## Key Findings

* **Short‑dated options (< 6 months)**: Both models produce comparable prices (errors < \$2).
* **Longer maturities (> 2 years)**:

  * Black‑Scholes errors grow significantly (up to \$25–30 for QQQ)
  * Binomial tree remains closer to market, likely due to discrete dividend handling
* **ETF-specific observations**:

  * QQQ’s higher volatility and tech‑sector skew amplify BS pricing error
  * DIA’s stability yields lower and more erratic errors for both models

---

## Dependencies

* Python ≥ 3.8
* numpy
* pandas
* scipy
* matplotlib
* yfinance

*All dependencies listed in* `requirements.txt`.

---

## Contributing

Contributions and suggestions are welcome. Please fork the repository and open a pull request with a clear description of your changes.

---

## License

This project is released under the [MIT License](LICENSE).
