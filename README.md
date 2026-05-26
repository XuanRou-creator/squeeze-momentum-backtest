# Squeeze Momentum Backtest System

A two-bucket backtesting framework for the Squeeze Momentum strategy, applied to Taiwan 50 ETF (0050). Compares six exit profiles against a Buy-and-Hold benchmark with full transaction cost modeling.

---

## Overview

The Squeeze Momentum indicator identifies periods when Bollinger Bands contract inside Keltner Channels (Squeeze ON), signaling a potential breakout. This project differentiates entry and exit logic based on whether the signal fires during a squeeze or after it releases.

**Two-bucket design:**

| Bucket | Signal | Entry Fraction | Logic |
|--------|--------|----------------|-------|
| Squeeze ON | Momentum positive / improving while squeezed | 25% of capital | Early / speculative entry |
| Squeeze OFF | Squeeze releases with upward momentum confirmation | 50% of capital | Confirmed trend entry |

---

## Exit Profiles Compared

Six exit strategies are backtested and compared simultaneously:

| Profile | Description |
|---------|-------------|
| Hold to End | No exit — baseline |
| Fixed TP / SL | ON: +20% / -15%，OFF: +35% / -18% |
| ATR Yo-Yo | Trailing stop anchored to highest high (ON: 4×ATR, OFF: 5×ATR) |
| Chandelier | Trailing stop from highest close (ON: 4×ATR, OFF: 5×ATR) |
| Breakeven | Dynamic stop that locks in breakeven after threshold, tightens on large gains |
| EMA Cross | Exit on death cross (ON: 8/21 EMA, OFF: 13/34 EMA) |

---

## Strategy Details

**Indicators**
- Bollinger Bands: 20-period, 2.0 std
- Keltner Channels: 20-period, 1.5× ATR
- Momentum: Linear regression of (close − midpoint) over 20 bars
- ATR: 14-period Wilder RMA

**Entry Signals**
- Squeeze ON entry: 3 consecutive squeeze bars + momentum positive/improving
- Squeeze OFF entry: Previous bar squeezed → current bar released + momentum positive and rising

**Transaction Costs (Taiwan market)**
- Buy fee: 0.1425%
- Sell fee: 0.1425% + tax 0.3%
- Lot size: 1,000 shares

**Add-on Logic**
- Additional lots executed only when price is at least 1×ATR above the current stop level

---

## Performance Metrics

Each exit profile is evaluated on:

- Total Return & Annualized Return
- Sharpe Ratio
- Max Drawdown
- Win Rate & Average Cycle Return
- Beta and Annualized Alpha vs Buy-and-Hold benchmark
- Executed entries, completed cycles, total transaction costs

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python |
| Data Processing | Pandas, NumPy |
| Visualization | Matplotlib |
| Data | 0050 OHLCV CSV (TWSE) |

---

## Project Structure

```
squeeze-momentum-backtest/
├── README.md
├── differentiated_entry_exit_buckets.ipynb   # Main backtest notebook
└── 0050_OHLCV.csv                            # Price data (not included — see below)
```

**Data:** Download 0050 historical OHLCV data from [TWSE](https://www.twse.com.tw) or any compatible source. Rename the file to `0050_OHLCV.csv` and place it in the root directory.

---

## How to Run

```bash
pip install numpy pandas matplotlib
jupyter notebook differentiated_entry_exit_buckets.ipynb
```

---

## Key Parameters

```python
INITIAL_CAPITAL = 1_000_000   # Starting capital (TWD)
LENGTH = 20                    # Bollinger / Keltner period
MULT = 2.0                     # Bollinger std multiplier
MULT_KC = 1.5                  # Keltner ATR multiplier
ATR_PERIOD = 14                # ATR period
```

---

*Applied to Taiwan 50 ETF (0050) as part of quantitative strategy research at NTHU.*
