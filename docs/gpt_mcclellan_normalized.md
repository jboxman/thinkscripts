# McClellan Timing + Confirmation

This guide documents [`gpt_McClellanNormalized.thinkscript`](../gpt_McClellanNormalized.thinkscript). The study combines the McClellan Oscillator (MCO) and McClellan Summation Index (MCSI) on one standard-deviation scale:

* **MCO is the timing tool.** It identifies short-term breadth stretch and washout conditions.
* **MCSI is the confirmation tool.** It identifies whether the longer-term breadth trend is repairing or deteriorating relative to its own moving average.

The default breadth universe is the components of the Nasdaq 100. The study is intended for a **daily chart**.

## Right-side blank space / chart expansion

The number of empty bars to the right is controlled by thinkorswim's chart settings, not by this study:

1. Open **Chart Settings** using the gear icon.
2. Select **Time axis**.
3. Enter the desired number in **Expansion area**.
4. Select **Apply** or **OK**.

The same time axis and expansion area apply to the main price chart and its lower study panes. The McClellan pane therefore aligns with the price chart's blank space automatically; no separate bar-count input is required.

By default, all MCO, MCSI, marker, and horizontal reference plots stop at the last real chart bar so the configured expansion area remains visibly blank. Set `extendReferenceLinesIntoExpansion` to `yes` if the ±1σ, ±2σ, and zero reference lines should continue across that blank area. MCO, MCSI, dots, and arrows never extend because doing so would imply future breadth readings that do not exist.

thinkScript cannot read or change the chart's current `Expansion area` setting. A similarly named bar-count input would only be a second, manually maintained number and could not keep itself synchronized with the chart. The chart setting supports up to 1,000 bars.

## Quick chart legend

| Chart element | Calculation or trigger | Meaning |
| --- | --- | --- |
| Green histogram bar | MCO score is at or above zero | Short-term breadth momentum is positive. |
| Red histogram bar | MCO score is below zero | Short-term breadth momentum is negative. |
| Cyan sloping line | MCSI distance from its 10dma, measured in residual standard deviations | Longer-term participation relative to its confirmation average. |
| Green dot on cyan line | Raw MCSI slope changes from falling/flat to rising | Early breadth repair, called an MCSI curl-up. |
| Yellow dot on cyan line | Raw MCSI slope changes from rising/flat to falling | Early participation deterioration, called an MCSI curl-down. |
| Green up arrow at center line | Raw MCSI crosses above its 10dma | Confirmed MCSI moving-average reclaim. |
| Red down arrow at center line | Raw MCSI crosses below its 10dma | Confirmed MCSI moving-average loss. |
| White center line | Zero sigma | MCO neutral and, separately, the exact MCSI/10dma cross level. |
| Dark-gray inner lines | +1σ and -1σ | Alert/context levels. |
| Gray outer lines | +2σ and -2σ | More statistically extended conditions. |

## Red and green histogram bars: MCO timing

The histogram is the normalized McClellan Oscillator. With the default `ZERO_CENTERED_SIGMA` method:

```text
MCO score = raw MCO / 63-day standard deviation of raw MCO
```

The bar's **color describes its side of zero**, not whether it rose or fell from yesterday:

* A **green bar** means the MCO score is zero or positive. Short-term breadth momentum is positive because the 19-day breadth EMA is at or above the 39-day breadth EMA.
* A **red bar** means the MCO score is negative. Short-term breadth momentum is negative because the 19-day breadth EMA is below the 39-day breadth EMA.
* The bar's **height** shows the magnitude in standard-deviation units. A bar at `-1.4` is a larger negative stretch than one at `-0.4`.

This means a shrinking red bar is still red even though breadth is improving. Likewise, a shrinking green bar remains green even though positive momentum is weakening. Read both the color and the change in height.

### MCO downside levels

* **Below -1σ:** the timing alert opens. Breadth is meaningfully washed out, but this is not by itself a reversal signal.
* **Below -2σ:** a deeper breadth washout. This can improve snapback potential, but severe declines can remain extended.
* **Back toward zero:** negative momentum is dissipating.
* **Above zero:** short-term breadth momentum has turned positive.

If `mcoNormalization` is changed to `Z_SCORE`, zero no longer means raw MCO neutral. It means raw MCO equals its recent 63-day average. The default zero-centered method is therefore better aligned with this timing-and-confirmation process.

## The cyan sloping line: MCSI confirmation

The cyan line is **not the raw cumulative MCSI**. It is the raw MCSI's distance from its 10-day moving average, scaled by the recent variability of that distance:

```text
MCSI residual = raw MCSI - 10dma(raw MCSI)

MCSI score = MCSI residual
             / 63-day standard deviation of historical MCSI residuals
```

This construction gives the line a precise interpretation:

* **Cyan above zero:** raw MCSI is above its 10dma. Breadth participation has confirmed positively.
* **Cyan below zero:** raw MCSI is below its 10dma. Longer-term breadth has not confirmed, or confirmation has been lost.
* **Cyan rising:** MCSI is improving relative to its 10dma. Participation is repairing or broadening.
* **Cyan falling:** MCSI is weakening relative to its 10dma. Participation is contracting.
* **Cyan near +1σ to +2σ:** MCSI is unusually far above its 10dma. This can represent strong participation; it is not automatically bearish or overbought.
* **Cyan near -1σ to -2σ:** MCSI is unusually far below its 10dma. The longer-term breadth trend is materially impaired or washed out.

The cyan line can fall while remaining above zero. That means confirmation is still intact, but its margin is narrowing. It can also rise while remaining below zero. That is early repair, but the 10dma has not yet been reclaimed.

## Green and yellow dots: early MCSI turns

Dots are plotted directly on the cyan line and identify the first reversal in the **raw MCSI's one-day slope**.

### Green dot — MCSI curl-up

A green dot appears when:

```text
raw MCSI is higher today
and raw MCSI was falling or flat yesterday
```

It means the longer-term breadth line has stopped declining and started to rise. This is an **early repair signal**, suitable for testing a turn or increasing attention. It is not the stronger moving-average confirmation.

Because cumulative MCSI changes each day by the amount of MCO, an MCSI curl-up is closely related to MCO turning positive. The dot should not be treated as fully independent confirmation.

### Yellow dot — MCSI curl-down

A yellow dot appears when:

```text
raw MCSI is lower today
and raw MCSI was rising or flat yesterday
```

It means participation has stopped improving and started to contract. In the described process, this is a caution signal: stop pressing new risk and see whether breadth stabilizes.

A yellow dot is more consequential when the cyan line is extended around +1σ to +2σ, because it may identify late-stage breadth deceleration. It is still not an automatic exit signal.

## Green and red arrows: confirmed MCSI crosses

Arrows are plotted at the white center line because the cyan line crosses zero at the exact moment raw MCSI crosses its moving average.

### Green up arrow — 10dma reclaim

A green arrow appears when raw MCSI crosses from below to above its 10dma. This is the stronger confirmation event:

* breadth repair has persisted beyond the first curl-up;
* the cyan MCSI score crosses above zero;
* participation has moved back above its confirmation average.

Within the process, this is the event that supports moving from a small test position toward greater conviction—provided price structure also agrees.

### Red down arrow — 10dma loss

A red arrow appears when raw MCSI crosses from above to below its 10dma:

* the cyan MCSI score crosses below zero;
* positive participation confirmation has been lost;
* adding new exposure becomes less attractive until breadth repairs again.

The arrow describes breadth structure, not price structure. It does not require an immediate liquidation of existing positions.

## Center, inner, and outer reference lines

The study displays five horizontal levels.

### White center line: 0σ

The center line has two simultaneous meanings because both indicators share the pane:

* For default MCO normalization, raw MCO equals zero—the 19-day and 39-day breadth EMAs are equal.
* For MCSI, raw MCSI equals its 10dma.

An MCO histogram zero-cross and an MCSI cyan-line zero-cross are therefore different events, even though they use the same visual level.

### Dark-gray inner lines: +1σ and -1σ

These are alert levels:

* `-1σ` is the first MCO washout/timing threshold.
* For MCSI, ±1σ means the index is one typical residual deviation away from its 10dma.

### Gray outer lines: +2σ and -2σ

These identify stronger statistical extensions:

* `-2σ` is the deeper MCO washout threshold.
* MCSI at `+2σ` or `-2σ` is unusually far from its 10dma relative to the last 63 sessions.

The upper levels are not automatic sell levels, and the lower levels are not automatic buy levels. Breadth indicators are autocorrelated and do not follow a perfect normal distribution; sigma is a normalization and context tool, not a probability guarantee.

## Typical signal sequences

### Constructive repair

1. Red MCO bars reach -1σ or -2σ.
2. Price begins retesting or reclaiming its 21-day structure.
3. Red MCO bars contract toward zero.
4. A green MCSI curl-up dot appears.
5. The cyan line rises toward zero.
6. A green arrow appears as MCSI reclaims its 10dma.
7. The cyan line remains above zero and rises while price structure holds.

The green dot is the early test; the green arrow is the stronger confirmation.

### Participation deterioration

1. The cyan MCSI line is positive, possibly extended toward +1σ or +2σ.
2. The cyan line begins to flatten.
3. A yellow curl-down dot appears.
4. MCO histogram bars weaken or turn red.
5. A red arrow appears if MCSI subsequently loses its 10dma.

The yellow dot is the warning; the red arrow confirms that the MCSI moving-average relationship has broken.

## What the study does not measure

MCO directly measures breadth momentum—not fear, sentiment, price support, or a guaranteed reversal. MCSI measures accumulated breadth and its relationship to its moving average—not whether the price index has reclaimed its 21dma. Those interpretations require the separate price-structure context described in the trading process.

## Default settings

| Input | Default | Purpose |
| --- | ---: | --- |
| `exchange` | `NASDAQ_100` | Uses Nasdaq 100 component advancers and decliners. |
| `fastLength` | `19` | Fast breadth EMA for MCO. |
| `slowLength` | `39` | Slow breadth EMA for MCO. |
| `ratioAdjusted` | `no` | Uses raw advancing minus declining NDX components. |
| `mcoNormalization` | `ZERO_CENTERED_SIGMA` | Preserves raw MCO zero as the neutral level. |
| `mcoSigmaLength` | `63` | Approximately one quarter of daily history for MCO scale. |
| `mcsiAverageLength` | `10` | MCSI confirmation moving average. |
| `mcsiAverageType` | `SIMPLE` | Implements the described 10dma. |
| `mcsiSigmaLength` | `63` | History used to scale MCSI/10dma residuals. |
| `alertLevel` | `1.0` | Inner ±1σ lines. |
| `washoutLevel` | `2.0` | Outer ±2σ lines. |
| `extendReferenceLinesIntoExpansion` | `no` | Stops horizontal levels at the last real bar, leaving chart expansion blank. |

Changing the lookbacks changes the meaning of every sigma reading. Keep settings stable when comparing different market cycles.
