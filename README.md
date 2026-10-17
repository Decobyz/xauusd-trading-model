# xauusd-trading-model
Quantitative research and backtesting of an algorithmic XAUUSD trading model using cTrader.
# Gold Adaptive Regime cBot: XAUUSD H4 backtest

A cTrader cBot for gold (XAUUSD) on the H4 chart. It trades moving-average crosses and pullbacks to the MA, with ATR-based stops and a fixed reward/risk target.

> **Status:** single backtest, one market regime, one 9-month window. Treat it as a starting point for discussion, not evidence of a live edge. See [Caveats](#caveats).

## Results (backtest)

| | |
|---|---|
| Period | 01 Jan 2026 to 08 Oct 2026 (9 months, 7 days) |
| Symbol / timeframe | XAUUSD, H4 |
| Starting capital | $5,000 |
| Ending balance | $7,427.17 |
| Net profit | $2,427.17 (+48.54%) |
| Total trades | 50 (21 long, 29 short) |
| Win rate | 58% (29 wins, 21 losses) |
| Profit factor | 2.47 (long 1.64, short 3.36) |
| Average trade | $48.54 |
| Average win / average loss | $140.48 / -$78.41 (payoff ratio 1.79) |
| Largest win / largest loss | $428.95 / -$226.28 |
| Max consecutive wins / losses | 11 / 8 |
| Max equity drawdown | 13.42% ($770.49) |
| Max balance drawdown | 11.08% ($629.41) |
| Average holding time | about 4.2 days (longest 16.3 days) |
| Costs paid | commissions -$12.00, swaps -$12.54 |

![Equity curve](results/equity_curve.png)

### Monthly P&L (by trade close date)

| Month | Trades | Net P&L |
|---|---|---|
| 2026-01 | 2 | +$250.16 |
| 2026-02 | 4 | -$91.53 |
| 2026-03 | 3 | +$292.31 |
| 2026-04 | 7 | +$712.44 |
| 2026-05 | 6 | -$55.55 |
| 2026-06 | 6 | +$343.78 |
| 2026-07 | 9 | -$325.41 |
| 2026-08 | 4 | +$403.02 |
| 2026-09 | 7 | +$888.82 |
| 2026-10 (partial) | 2 | +$9.13 |

## Test setup

| | |
|---|---|
| Platform | cTrader Algo, historical replay (backtest) |
| Data | M1 bars from the broker's server (Pepperstone) |
| Account | Hedging, 1:400 leverage, USD |
| Spread | Fixed, value 1 |
| Commission | $30 per million USD volume |
| Trade size | Minimum volume (0.01 lot = 1 oz) on every trade |

## Strategy parameters

Optional filters not listed as "on" below were switched **off** for this run.

| Parameter | Value |
|---|---|
| Gold MA period | 50 (on the chart timeframe; higher-timeframe MA off) |
| Signal window after cross | 3 bars |
| Max chase distance | 1.0 ATR from the MA |
| Pullback entries | On, zone 0.5 ATR from the MA, MA slope lookback 5 bars |
| Max open positions | 2 (at least 5 bars between extra entries) |
| ATR period | 20 |
| Stop loss | 1.5 x ATR |
| Reward / risk | 2.0 |
| Risk per trade (parameter) | 1% |
| Max spread | 10% of stop |
| Off in this run | higher-timeframe MA, cross buffer, reverse on opposite cross, equity divergence filter (US500), session filter, hard spread cap |

Full parameter list and raw numbers: see `results/backtest_report.html`.

## Caveats

- **Short sample.** 50 trades over about nine months is a small sample, and it comes from one period of gold's history.
- **Concentrated profits.** The five best trades account for about 53% of net profit. Without the top three, net profit is about $1,532.
- **Long/short asymmetry.** Shorts made $1,885 (PF 3.36) and longs $542 (PF 1.64). That may say more about 2026's market than about the strategy.
- **Fixed spread.** The test assumed a fixed spread of 1. Live gold spreads vary and widen around news, so real costs will likely be higher.
- **Position sizing.** Every trade used the minimum volume, so the 1% risk parameter was not what set trade size here. The largest loss (-$226.28) was about 4.5% of starting capital.
- **No out-of-sample test yet.** Parameters were not validated on data outside this window.
- **Backtests are not live results.** Slippage, requotes, and execution differences are not modelled.

*This is not financial advice. Past performance does not guarantee future results. Use at your own risk.*

## Repo layout

```
.
├── README.md
├── src/                 # cBot source (.cs)
├── params/              # .cbotset used for this run
└── results/
    ├── backtest_report.html
    └── equity_curve.png
```

## License

Add a license of your choice (e.g. MIT) before making the repo public.<img width="808" height="278" alt="a2" src="https://github.com/user-attachments/assets/72601b78-c88a-40eb-a632-618c7ee2f37e" />
<img width="913" height="454" alt="a1" src="https://github.com/user-attachments/assets/a68c0497-0916-4cb9-b1b4-166960ff891f" />
