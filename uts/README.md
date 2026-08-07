# UseThinkScript Studies

## Ten Closes / 5 SMA Strategy

Script: [`uts_strategy_ten_closes_5sma.thinkscript`](uts_strategy_ten_closes_5sma.thinkscript)

This daily-bar strategy enters long after 10 consecutive closes above the
5-period SMA, or short after 10 consecutive closes below it. It enters only on
the bar where the streak first reaches 10, ignores additional signals while a
trade is open, and exits after 10 bars. The lengths, direction, fill model,
contract size, point value, slippage, commissions, and date range are inputs.

The default fill model evaluates the setup at the close and fills at the next
bar's open. Select `signalBarClose` to model the source rule literally. For a
ten-year test, use a `10Y:1D` chart or enable the strategy's date filter.

For futures, verify the displayed automatic point value before relying on the
custom P/L labels. It can be overridden when symbol metadata is unavailable or
does not match the contract being tested.

## VVIX/VIX Ratio

Script: [`uts_vvix_vix.thinkscript`](uts_vvix_vix.thinkscript)

### Purpose

The VVIX/VIX Ratio study compares the market's expected volatility of VIX
with the market's expected volatility of the S&P 500:

- **VIX** reflects expected 30-day S&P 500 volatility derived from SPX options.
- **VVIX** reflects expected volatility of VIX derived from VIX options.

The study plots:

```text
VVIX/VIX ratio = VVIX close / VIX close
```

For example, a VVIX value of 90 and a VIX value of 15 produce a ratio of 6.
The ratio is a relative measure: it describes how elevated VVIX is compared
with VIX, not whether either index is high or low in absolute terms.

### What moves the ratio

The ratio can rise when:

- VVIX rises faster than VIX.
- VVIX remains steady while VIX falls.
- Both indexes fall, but VIX falls faster.

The ratio can fall when:

- VIX rises faster than VVIX.
- VIX remains steady while VVIX falls.
- Both indexes rise, but VIX rises faster.

This distinction matters because the same ratio reading can occur under very
different market conditions. A high ratio caused by unusually low VIX is not
equivalent to a high ratio caused by rapidly rising VVIX. Similarly, a low
ratio can occur during a volatility shock if VIX rises faster than VVIX.

### Display and default reference levels

The study appears in a lower chart pane and includes:

| Element | Default | Meaning |
| --- | ---: | --- |
| Yellow line | Current ratio | VVIX divided by VIX |
| Red dashed line | 9.0 | Configurable upper reference |
| Green dashed line | 6.0 | Configurable lower reference |
| Label | Current ratio | Rounded to two decimal places |

The label color describes the ratio's position relative to the configured
levels:

- **Red:** above the upper threshold.
- **Yellow:** between the two thresholds.
- **Green:** below the lower threshold.
- **Gray:** ratio data is unavailable.

These colors identify relative zones only. They should not be read as direct
sell, neutral, or buy signals.

### Interpretation

The indicator is best used as a context or regime tool:

- A rising ratio indicates VVIX is strengthening relative to VIX.
- A falling ratio indicates VIX is strengthening relative to VVIX.
- A move through a reference level highlights a relative extreme based on the
  chosen thresholds.

Before drawing a conclusion, check the direction and absolute level of both
VIX and VVIX. Price trend, market breadth, index behavior, and the broader
volatility term structure can also help distinguish a quiet-market anomaly
from an active volatility event.

The default levels of 6 and 9 are reference points, not universal boundaries.
Their usefulness can vary with the chart aggregation, time period, and market
regime. They can be changed in the study settings:

- `upperThreshold`: upper reference level; default `9.0`.
- `lowerThreshold`: lower reference level; default `6.0`.
- `vixSymbol`: VIX symbol; default `"VIX"`.
- `vvixSymbol`: VVIX symbol; default `"VVIX"`.

### Data handling

The script uses the close of each secondary symbol at the chart's aggregation
period. It plots `Double.NaN` instead of a ratio when VIX or VVIX data is
missing, or when VIX is not greater than zero. This prevents invalid division
and gaps the plot when a usable calculation is unavailable.

Secondary-symbol data availability can differ by account permissions,
instrument session, and aggregation. Missing or asynchronous quotes may
therefore cause temporary gaps or readings that differ across timeframes.

### Limitations

- The ratio does not reveal whether VIX and VVIX are rising or falling without
  inspecting the component indexes.
- A threshold crossing does not predict the direction or timing of an equity
  market move.
- The reference levels are static and do not adjust automatically to changing
  volatility regimes.
- The ratio is not a standardized probability, percentile, or risk score.
- It should not be used as a standalone trading signal.
