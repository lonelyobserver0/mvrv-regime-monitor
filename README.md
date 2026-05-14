# MVRV Regime Monitor

An honest Bitcoin MVRV indicator for TradingView. **Validated thresholds, falsified scope, disclosed limits.**

---

## What this is

A Pine Script v5 indicator that:

- Pulls `INTOTHEBLOCK:BTC_MVRV` (free TradingView feed) on the daily timeframe
- Shows the current MVRV value with four regime thresholds (entry, accumulation, exit, euphoria)
- Colors the background by regime so you can see *at a glance* whether Bitcoin is in a value zone, neutral zone, or distribution zone
- Fires alerts when MVRV crosses any of the validated thresholds
- States its statistical limits clearly in tooltips on every input

## What this is NOT

- **Not a buy/sell signal generator.** It tells you where in the cycle BTC sits. You decide what to do.
- **Not a high-frequency strategy.** N=3 trades in 8 years.
- **Not applicable to altcoins.** Falsified on 8 of them (see below).
- **Not a guaranteed edge forward.** Cycle Sharpe has been declining (1.96 → 1.81 → 1.40); ETF maturity appears to be compressing the signal.

If you want hype, there are dozens of other MVRV indicators on TradingView. This one is what's left after stripping the marketing.

---

## Why this exists

After spending six months testing rule-based, ML-based, and on-chain signal strategies on crypto majors — and **falsifying all of them except this** — I wanted one TradingView indicator that does what the others don't: state the actual statistical evidence, not the marketing version.

Every MVRV indicator on TradingView claims it identifies cycle tops and bottoms. None of them disclose:

- The walk-forward Sharpe
- The number of trades the signal has actually produced
- Whether the signal holds on altcoins (it doesn't)
- Whether the edge is decaying (it is, since ETF launch)

This one does.

---

## Walk-forward validation

Methodology: in-sample 2018-01 to 2021-12, out-of-sample 2022-01 to 2026-04. 10 bps trading fee. Long when MVRV crosses below entry threshold, exit when MVRV crosses above exit threshold.

### Threshold sweep (64 combinations tested)

| Outcome | Result |
|---|---|
| Best IS combo | low=1.0, high=2.5 |
| IS CAGR @ best combo | +76%, Sharpe 1.62 |
| **OOS CAGR @ same combo** | **+27%, DD −36%, Sharpe 0.91** |
| OOS trades | 1 (entered 2022-11, exited 2024-02) |
| Threshold combos with positive CAGR | 64/64 |
| Threshold combos beating buy & hold | 52/64 |

### Full period (2018-2026) at validated thresholds

| Metric | MVRV strategy | Buy & Hold |
|---|---|---|
| CAGR | **+48%** | +24% |
| Max drawdown | **−36%** | −81% |
| Time invested | 41% | 100% |

The edge isn't *return enhancement* (BTC buy & hold beats most things over long horizons). The edge is **drawdown reduction**: roughly half the max DD at twice the CAGR, by sitting in cash for ~60% of the cycle.

---

## Multi-asset falsification

The same strategy tested on 8 altcoins (ETH, LTC, BCH, BNB, ADA, XRP, DOT, LINK):

| Asset | Strategy CAGR | Buy & Hold | Verdict |
|---|---|---|---|
| BTC | +48% | +24% | ✓ Robust |
| BCH | +12% | −9% | ✓ (B&H is terrible anyway) |
| ETH | −2% | +18% | ✗ Underperforms |
| LTC | −7% | −12% | ≈ Beats B&H but both bad |
| BNB | +4% | +71% | ✗ Massively underperforms |
| ADA | −9% | +14% | ✗ Loses money |
| XRP | +1% | +8% | ✗ Underperforms |
| DOT | −31% | −47% | ≈ Beats B&H but both bad |
| LINK | +2% | −18% | ≈ Beats B&H but B&H is terrible |

Correlations between MVRV level and forward returns are **positive or zero** on most altcoins (anti-pattern). The MVRV mean-reversion signal that works on BTC is not generalizable.

**Conclusion: this is a BTC-only model.** Use it on the BTC chart; do not extrapolate.

---

## Cycle-over-cycle decay

Sub-period Sharpe ratios for the same thresholds:

| Cycle window | Sharpe |
|---|---|
| 2018-2021 | 1.96 |
| 2020-2023 | 1.81 |
| 2022-2026 | 1.40 |

The drop is **consistent with structural ETF impact**: spot ETF launch in January 2024 added a large pool of capital with different behavioral patterns than retail/OG holders, smoothing the cycle peaks and troughs that MVRV traditionally exploited.

**Realistic forward expectation:** CAGR +20% to +40%, max DD −30% to −40%, Sharpe 1.0 to 1.4. Not the pre-ETF numbers.

---

## How to use

1. Open TradingView and load any BTC chart (e.g., `BINANCE:BTCUSDT`)
2. Open the Pine Editor → paste contents of [`mvrv_regime_monitor.pine`](./mvrv_regime_monitor.pine)
3. Save → Add to chart
4. The indicator appears in a separate pane below price
5. (Optional) Right-click the indicator → Add Alert → pick one of the three threshold alerts

The indicator pulls MVRV automatically from INTOTHEBLOCK regardless of which BTC ticker your main chart uses. Daily granularity.

---

## Paid edition (in development)

A separate Invite-Only version is in development with:

- **Configurable threshold alerts** (custom values, not just the defaults)
- **Precise historical cycle annotations** computed live from the MVRV series
- **MVRV Z-score normalized variant** (more stable for long-horizon decisions)
- **Cycle-by-cycle Sharpe tracker** showing whether the edge is decaying further
- **Discord access** for Q&A on the methodology

Expected price: $10/month or $50 one-time. If you're interested, open an issue on this repo and I'll DM you when it ships.

---

## Caveats and risk disclosure

- This is **research-quality work**, not investment advice. The N=3 trade sample is statistically thin.
- ETF-driven compression of the edge may continue. The next entry trigger (MVRV < 1.0) may produce a smaller move than past triggers.
- The model has no opinion on geopolitical, regulatory, or exchange-counterparty risk.
- Position sizing, leverage, and exit discipline are your responsibility.
- Cryptocurrency trading involves substantial risk of total loss. Use stop losses. Do not invest money you cannot afford to lose.

---

## License

MIT. See [LICENSE](./LICENSE). Use commercially, fork freely, attribution appreciated.

---

## Author

**lonelyobserver0** — independent crypto research. Reach via TradingView DM or GitHub issues.
