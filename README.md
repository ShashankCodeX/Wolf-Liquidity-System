# Wolf Liquidity System

A compact TradingView Pine Script indicator for 5-minute scalping on XAUUSD, BTCUSDT, and other liquid markets. It maps nearby liquidity, highlights confirmed sweep-and-reclaim candles, and keeps ordinary volume in its own pane.

> **Use on a standard candlestick chart.** The default settings are a starting point for 5-minute charts; tune them for the symbol, feed, and session you trade.

## What it shows

- **Buy-side and sell-side liquidity:** confirmed pivot highs and lows, with the most recent active levels extended to the right.
- **Equal highs / equal lows:** pivots close together by an ATR-based tolerance are marked `EQH` or `EQL`.
- **Higher-timeframe reference levels:** previous day high/low (`PDH` / `PDL`) and previous week high/low (`PWH` / `PWL`). These use completed daily and weekly candles.
- **Session range:** the latest selected session high and low. The default window is 00:00–05:00 New York time; the lines update while that session is open and remain until the next one starts.
- **Sweep/reclaim markers:** `SSL` appears when price trades below a confirmed sell-side level and closes back above it. `BSL` appears when price trades above a confirmed buy-side level and closes back below it. Optional relative-volume confirmation is available.
- **Volume pane:** standard chart volume, a configurable volume average, and above-average volume coloring.
- **Optional fair value gaps:** unfilled three-bar bullish and bearish gaps, disabled by default to keep the chart clean.
- **Optional native footprint readout:** bar delta, point of control, and detected row imbalances. Enable this only if your TradingView plan supports footprint requests.

## Install

1. Open [TradingView Pine Editor](https://www.tradingview.com/pine/).
2. Copy the contents of [`Wolf-Liquidity-System.pine`](Wolf-Liquidity-System.pine) into a new script.
3. Save it as **Wolf Liquidity System**, then choose **Add to chart**.
4. Set the chart to **5 minutes** and a standard candlestick chart. Adjust the session time zone if needed.
5. To create alerts, choose one of the script conditions: **Bullish SSL sweep** or **Bearish BSL sweep**. Alerts trigger on confirmed bar closes.

The script uses `overlay = false` so volume has its own pane. Liquidity lines, session levels, FVGs, sweep markers, and footprint POC dots are individually drawn on the price chart.

## Suggested 5-minute starting settings

| Setting | Default | Purpose |
| --- | ---: | --- |
| Pivot confirmation bars | 3 | Confirms a swing with three bars on each side. A pivot appears after confirmation, so it is intentionally delayed. |
| Active swing levels per side | 8 | Limits chart clutter while retaining recent pools. |
| Equal-level tolerance | 0.10 ATR | Groups nearby confirmed pivots as a possible equal-high/low pool. |
| Volume average | 20 bars | Context for relative volume and the volume pane. |
| Relative-volume filter | Off | When enabled, sweep markers require volume at or above the chosen multiple of the average. |
| FVG zones | Off | Optional context; old zones are removed after price fully trades through them. |
| Footprint | Off | Optional native TradingView footprint data, requiring Premium or Ultimate. |

## Reading the signals

- A swing high is a potential **buy-side liquidity** reference; a swing low is a potential **sell-side liquidity** reference. These are chart-derived areas, not a record of actual resting orders.
- `EQH` and `EQL` identify nearby swing levels that traders may watch as a liquidity pool. The tolerance is measured against ATR at the pivot.
- A sweep marker means the candle wicked through one or more active swing levels, then closed back on the original side. It is a rejection event to review in context, not an automatic entry instruction.
- Close-throughs retire a swing level without generating a sweep marker. Retired lines and their tags are deleted to keep the chart and object count limited.
- The relative-volume filter uses the chart's `volume` series. It is optional because the meaning and quality of volume differ by market and data feed.
- The footprint panel reports the current bar's footprint delta and POC and whether any footprint row has a buy or sell imbalance. The POC dot marks the current bar's POC price.

## Inputs

All settings are grouped in the indicator's Inputs tab:

- **Liquidity:** pivot confirmation, number of active levels, swing tags, equal-level detection, and tolerance.
- **Higher-timeframe levels:** previous day and previous week highs/lows.
- **Session range:** time window and one of New York, UTC, or India time.
- **Sweep signals:** optional relative-volume requirement and threshold.
- **Volume:** average length and above-average highlighting.
- **Fair value gaps:** visibility and maximum zones per direction.
- **Order-flow footprint:** enable switch, ticks per row, value-area percentage, and imbalance threshold.

## Important behavior and limitations

- Swing pivots require right-side bars to confirm. They are plotted back at the pivot candle only after confirmation; the indicator does not know the swing in advance.
- Sweep alerts are evaluated at candle close. Live, unfinished candles can move through a level before the final close confirms or rejects it.
- Daily and weekly levels use the prior completed candle, avoiding a moving current-day/current-week reference.
- Native footprint requests are available only on TradingView **Premium and Ultimate** plans. Leave **Enable native footprint data** off if your plan does not include it; the core liquidity and volume features do not depend on footprint data.
- TradingView footprint data classifies intrabar volume into buy/sell sides based on its feed and footprint calculation. It is not a full exchange order book or a guarantee of the aggressor behind every trade.
- XAUUSD feeds commonly report tick volume rather than centralized traded volume. BTCUSDT volume and footprint data are specific to the selected exchange/symbol feed, not the entire crypto market.
- This is an indicator, not a strategy or financial advice. It does not place orders, size positions, set stops, or establish profitability. Validate signals and risk controls on your own data before live use.

## Public repository details

**Suggested GitHub description:**

> Minimal TradingView Pine Script indicator for 5-minute scalping: swing liquidity, EQH/EQL, PDH/PDL, session ranges, sweep/reclaim alerts, volume, optional FVGs and native footprint data.

**Suggested topics:** `tradingview`, `pine-script`, `pine-script-v6`, `liquidity`, `ict`, `smart-money-concepts`, `volume`, `footprint`, `xauusd`, `btc-usdt`, `scalping`

## Sources

- [Pine Script v6 User Manual](https://www.tradingview.com/pine-script-docs/)
- [Other timeframes and data: volume footprints](https://www.tradingview.com/pine-script-docs/concepts/other-timeframes-and-data/)
- [Visuals overview: overlaying individual visuals](https://www.tradingview.com/pine-script-docs/visuals/overview/)
