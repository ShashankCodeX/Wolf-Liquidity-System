# Wolf Liquidity System

A minimal TradingView Pine Script v6 indicator for 5-minute scalping on XAUUSD, BTCUSDT, and other liquid markets. It highlights selected liquidity levels, confirmed sweep-and-reclaim candles, and symbol volume.

The default layout focuses on equal highs/lows, the previous day’s high/low, and one session range. Optional previous-week levels, fair value gaps, and footprint data are off by default.

> This indicator helps organize chart information. It does not guarantee a reversal or an 80% win rate. Any win-rate claim needs to be measured with a defined strategy and tested on the symbol, feed, time period, and costs you actually trade.

## Quick start

1. Open the Pine Script in TradingView’s Pine Editor.
2. Add the script to a standard candlestick chart.
3. Set the chart to 5 minutes.
4. Open the indicator’s **Settings → Inputs** to adjust levels, volume, session times, or colors.
5. To create alerts, choose **Bullish SSL sweep** or **Bearish BSL sweep** as the alert condition.

The volume plot appears in its own pane. Liquidity lines, sweep markers, session lines, optional FVGs, and the optional footprint POC are drawn on the price chart.

## Chart legend

| Chart item                             | Meaning                                                                                                                    |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Gold `EQH`                             | Two consecutive confirmed swing highs that fall within the selected ATR tolerance. A possible pool of buy-side liquidity.  |
| Gold `EQL`                             | Two consecutive confirmed swing lows that fall within the selected ATR tolerance. A possible pool of sell-side liquidity.  |
| Rose `BSL` swing level                 | A confirmed swing high. It is a potential buy-side liquidity reference above price, not a record of actual resting orders. |
| Teal `SSL` swing level                 | A confirmed swing low. It is a potential sell-side liquidity reference below price, not a record of actual resting orders. |
| Blue `PDH` / `PDL` lines               | The previous completed day’s high and low.                                                                                 |
| Lavender `PWH` / `PWL` lines           | The previous completed week’s high and low. Hidden by default.                                                             |
| Dotted session lines                   | The high and low of the selected session. The default range is 00:00–05:00 New York time.                                  |
| `BSL` marker above a candle            | Price traded above a tracked buy-side level and closed back below it. This is a bearish sweep-and-rejection event.         |
| `SSL` marker below a candle            | Price traded below a tracked sell-side level and closed back above it. This is a bullish sweep-and-reclaim event.          |
| Lower volume pane                      | Symbol volume as columns, plus a 20-bar simple moving average.                                                             |
| Optional pale boxes                    | Three-bar fair value gaps. The boxes are removed when price fully trades through them.                                     |
| Optional `WOLF FLOW` panel and POC dot | Current-bar footprint delta, point of control (POC), and detected row-imbalance information.                               |

When **Only show EQH / EQL pools** is enabled, ordinary swing levels are hidden. The default script shows at most two active equal-high pools and two active equal-low pools.

## How the indicator works

### Swing pivots and equal highs/lows

A pivot must be confirmed by bars on both sides. The default is five bars on each side, so the indicator waits for confirmation before drawing a level. It does not predict the swing in advance.

The script compares each new confirmed pivot with the previous confirmed pivot on the same side. If the difference is within the selected ATR tolerance, it marks the new level as `EQH` or `EQL`. The default tolerance is 0.15 ATR.

### Sweep-and-reclaim markers

A bullish `SSL` marker appears when price moves below an active sell-side level and the candle closes back above it. A bearish `BSL` marker appears when price moves above an active buy-side level and closes back below it.

The script can check equal-high/low pools, the previous day’s high/low, and the completed session range. Previous-week levels are also included in sweep checks if you enable them.

A close through a tracked swing level removes that level without marking it as a sweep. Sweep markers are evaluated on confirmed candle closes.

### Relative volume

The relative-volume filter is on by default. A sweep marker requires volume to be at least 1.20 times the selected volume average. The default average length is 20 bars.

This filter can reduce the number of markers, but it does not measure the probability that a trade will win. Volume is taken from the chart’s data feed. For many XAUUSD feeds, it may represent tick volume rather than centralized traded volume. Crypto volume depends on the selected exchange and symbol.

### Previous-day and previous-week levels

`PDH` and `PDL` come from the previous completed daily candle. `PWH` and `PWL` come from the previous completed weekly candle. The weekly levels are hidden by default; enable them in **Settings → Inputs → Higher-timeframe levels** if you want them.

### Session range

The default range is 00:00–05:00 New York time. You can change the session hours and select New York, UTC, or India time in the **Session range** settings. The high and low update while the selected session is open and remain on the chart until the next selected session starts. Session sweeps are checked after the range-building session closes.

### Volume

The volume bars use the chart symbol’s normal TradingView volume series. Up bars and down bars have separate colors. When **Highlight above-average volume** is enabled, bars at or above the volume average are emphasized.

This volume display is separate from the relative-volume filter for sweep markers. The filter’s default threshold is 1.20 times average volume; the bar-color highlight begins at the average.

### Optional fair value gaps

Fair value gaps (FVGs) are disabled by default. When enabled, the script marks three-bar bullish and bearish gaps. A zone is removed after price fully trades through it. The number of zones per direction is limited by the FVG settings.

### Optional footprint data

Footprint data is disabled by default. When enabled and available, the script displays the current bar’s delta, POC, and whether footprint rows show buy or sell imbalance. It is a compact current-bar readout, not a full footprint ladder, exchange-wide order book, or guarantee of the aggressor behind a trade. Availability depends on the TradingView plan and data feed.

## Default settings

| Setting                      |   Default | Purpose                                                                                 |
| ---------------------------- | --------: | --------------------------------------------------------------------------------------- |
| Pivot confirmation bars      |         5 | Waits for a larger confirmed swing; more bars mean fewer levels and later confirmation. |
| Active pools per side        |         2 | Limits the number of displayed equal-high or equal-low pools on each side.              |
| Only show EQH / EQL pools    |        On | Hides ordinary swing levels to keep the chart minimal.                                  |
| Equal-level tolerance        |  0.15 ATR | Sets how close consecutive pivots must be to count as equal.                            |
| Previous day high / low      |        On | Shows the previous completed day’s high and low.                                        |
| Previous week high / low     |       Off | Adds longer-term reference levels when enabled.                                         |
| Session range                |        On | Shows the selected session’s high and low.                                              |
| Relative-volume confirmation | On, 1.20× | Requires a sweep candle to meet the selected volume threshold.                          |
| Volume average               |   20 bars | Moving average used in the volume pane and relative-volume calculation.                 |
| FVGs                         |       Off | Optional fair value gap zones.                                                          |
| Footprint                    |       Off | Optional TradingView footprint readout.                                                 |

## Customize the colors

Open the indicator’s **Settings → Inputs → Colors - light chart**. The color inputs control:

- Bullish levels, SSL markers, and bullish FVGs
- Bearish levels, BSL markers, and bearish FVGs
- Equal highs and lows
- Previous-day and previous-week levels
- Session range
- Up-volume bars, down-volume bars, and the volume average
- Footprint POC and panel colors
- Sweep-marker text

The defaults use a light palette intended for a white chart. Change the inputs to match your own chart theme.

## Alerts

Create an alert from the indicator and select one of these conditions:

- **Bullish SSL sweep** — sell-side liquidity was swept and reclaimed.
- **Bearish BSL sweep** — buy-side liquidity was swept and rejected.

The conditions use confirmed candle closes. They are notifications to review the chart, not instructions to enter a trade.

## Limitations and risk

- A liquidity level is a chart-derived reference. The indicator does not know where all real market orders or stop-losses are resting.
- A sweep marker describes a price pattern; it does not prove that the larger trend has reversed.
- Pivots are confirmed after the selected number of right-side bars, so levels appear with a delay.
- Results can differ between brokers, exchanges, and symbols because their price and volume feeds differ.
- The indicator does not place orders, set stops or targets, size positions, or calculate profitability.
- No win rate is promised. To evaluate a setup, define entry, stop, target, spread, slippage, and trading hours, then test those rules on historical data and forward-test them on your feed.

## Install or update the script

1. Open the repository’s `Wolf-Liquidity-System.pine` file.
2. Copy the Pine code into TradingView’s Pine Editor. Do not include Markdown code-fence lines.
3. Save the script and add it to a 5-minute chart.
4. Adjust the settings to match your symbol, feed, and session.

## Push changes to GitHub

From a terminal opened in the repository folder, stage both files, commit them, and push to `main`:

`git add Wolf-Liquidity-System.pine README.md`

`git commit -m "Simplify liquidity levels and update documentation"`

`git push origin main`

## Suggested GitHub description

Minimal TradingView Pine Script indicator for 5-minute scalping: equal-high/low liquidity pools, previous-day and session levels, sweep-and-reclaim alerts, volume, optional FVGs and footprint data.

**Suggested repository topics:** `tradingview`, `pine-script`, `pine-script-v6`, `liquidity`, `ict`, `smart-money-concepts`, `volume`, `footprint`, `xauusd`, `btc-usdt`, `scalping`

## Sources

- [Pine Script v6 User Manual](https://www.tradingview.com/pine-script-docs/)
- [Pine Script inputs](https://www.tradingview.com/pine-script-docs/concepts/inputs/)
- [Volume footprints](https://www.tradingview.com/pine-script-docs/concepts/other-timeframes-and-data/)
- [Visuals and overlays](https://www.tradingview.com/pine-script-docs/visuals/overview/)
