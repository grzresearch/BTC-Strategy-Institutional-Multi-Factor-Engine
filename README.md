# BTC Strategy: Institutional Multi-Factor Engine

A multi-factor engine for BTC, written as a Pine Script indicator for
TradingView. It brings an institutional-grade analytical framework to Bitcoin
trading by combining technical price action with deeper market drivers such
as Open Interest, volume flow and macro correlations.

## Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Alerts](#alerts)
- [Limitations](#limitations)
- [Project structure](#project-structure)
- [License](#license)

## Overview

Unlike standard indicators that often rely on a single metric, this tool uses
an ensemble machine learning engine that aggregates and weights factors
ranging from momentum and mean reversion to cross-market data to generate
high-probability entry signals. To keep these signals robust against typical
crypto volatility, the raw signal is processed through an adaptive Kalman
filter rather than traditional moving averages, which gives a clearer view of
the underlying trend.

What distinguishes the tool is its built-in paper trading simulation. While it
technically runs as an indicator, it tracks hypothetical trade execution in
the background and shows real-time performance metrics such as win rate,
profit factor and drawdown on the dashboard, giving immediate insight into the
strategy's current effectiveness without a separate backtest.

The system also integrates risk management modules, including liquidation
cascade detection, which temporarily blocks signals during violent sell-offs
to avoid catching "falling knives", and dynamic signal thresholds that adjust
to market stress. By placing price action in the context of the 4-year
halving cycle and filtering out low-liquidity weekend noise, the strategy
aims to align short-term execution with the broader market regime: a
complete, data-driven trading system in a single chart overlay.

## Features

The script is organized into input groups that can each be enabled or
disabled in the settings dialog:

| Module | What it does |
|--------|--------------|
| Weekend liquidity protection | reduces the suggested position size and suppresses entry signals from the Friday exit hour until the Sunday re-entry hour |
| 4-year halving cycle | classifies the days since the last halving (default 2024-04-20) into accumulation, bull, distribution and bear phases |
| Cross-market screening | correlations with S&P 500, DAX and Hang Seng, plus VIX and DXY risk-off filters |
| Open interest flow | OI momentum, divergence and OI flips from a proxy built on a perpetual-futures symbol (default `BINANCE:BTCUSDTPERP`) |
| Spot flow analysis | volume delta, CVD, large-order and absorption detection |
| Treasury holdings proxy | momentum and lead signals from MSTR, COIN and GBTC |
| ML signal engine | combines the normalized factor scores with one of three ensemble methods: Weighted Average, Voting or Stacking |
| Kalman filter | smooths the raw signal, with optional adaptive noise estimation |
| Dynamic thresholds | moves the entry thresholds with volatility and signal quality |
| Information coefficient | tracks the signal's IC over two horizons and filters signals when it degrades |
| CVaR risk engine | caps the suggested position size using the conditional value at risk (95% by default) |
| Liquidation cascade | detects volume spikes with large price moves and blocks signals for a cooldown period |
| Risk management | ATR-based stop loss and take profit, maximum drawdown stop, Kelly-fraction position size |
| Regime detection | ADX trend strength, volatility regime and mean-reversion state |
| Technical analysis | EMA 21 / 55 / 200, RSI, MACD, Bollinger Bands |

On the chart the indicator draws the EMAs, VWAP and Bollinger Bands, long,
short and exit markers, OI flip markers, liquidation warnings and a regime
background. The analytics dashboard shows the state of every module, the
filter status and the simulated performance.

## Installation

The script requires Pine Script v6 (`//@version=6`).

1. Open a BTC chart on [TradingView](https://www.tradingview.com) and open
   the **Pine Editor**.
2. Replace the editor contents with
   [`BTC_Strategy_Institutional_Multi-Factor_Engine.pine`](BTC_Strategy_Institutional_Multi-Factor_Engine.pine).
3. Click **Save**, then **Add to chart**.

## Usage

The indicator is an overlay named **BTC Strategy**. After adding it to a
chart, open its settings to switch modules on or off, change the reference
symbols (S&P 500, DAX, Hang Seng, DXY, VIX, the perpetual used for OI, MSTR,
COIN, GBTC) and adjust thresholds and risk parameters. The dashboard position
can be set to any corner of the chart.

## Alerts

Create an alert on the indicator and choose one of these conditions:

| Condition | Message |
|-----------|---------|
| Long Entry Signal | BTC Strategy: LONG Entry |
| Short Entry Signal | BTC Strategy: SHORT Entry |
| Exit Long Signal | BTC Strategy: EXIT LONG |
| Exit Short Signal | BTC Strategy: EXIT SHORT |
| OI Flip Bullish / OI Flip Bearish | BTC: OI Flip BULLISH / BEARISH |
| Weekend Protection | BTC: Weekend Protection ACTIVATED |
| Max Drawdown | BTC ALERT: Max Drawdown Breached |
| Risk-Off Signal | BTC: Risk-Off Detected |
| Liquidation Cascade | BTC Strategy: Liquidation Cascade Detected! |
| IC Degraded | BTC Strategy: IC Degraded - Signal Quality Poor |

## Limitations

- The performance figures on the dashboard come from the built-in paper
  trading simulation, which starts from a simulated equity of 100,000 and does
  not model fees or slippage. They are not a substitute for a full backtest.
- The open interest module does not use exchange-reported open interest: its
  OI proxy is a moving average of the configured perpetual symbol's volume.
- The inputs *Enable Trailing Stop* and *Trailing Stop ATR Multiplier* are
  not used by the current version of the script.
- Cross-market, OI and treasury factors depend on the data TradingView
  provides for the configured symbols on the chart's timeframe.
- In the ML signal engine the factor weights depend on the detected regime
  and are normalized to sum to one.

## Project structure

```
btc-strategy-institutional-multi-factor-engine/
├── BTC_Strategy_Institutional_Multi-Factor_Engine.pine   # the indicator (Pine Script v6)
├── README.md
└── LICENSE                                               # Mozilla Public License 2.0
```

## License

Released under the Mozilla Public License 2.0, see [LICENSE](LICENSE).
