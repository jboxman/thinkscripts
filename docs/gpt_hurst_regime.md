# Rolling Hurst Regime Proxy

`gpt/gpt_hurst_regime.thinkscript` estimates how the root-mean-square (RMS)
displacement of log price scales across 1, 2, 4, 8, and 16 bars over a rolling
window. RMS avoids the additional finite-window bias caused by subtracting a
separate sample mean from each set of overlapping multi-bar changes.

If RMS displacement at lag `k` behaves approximately as `k^H`, then:

```text
log(RMS(k-bar log-price change)) = constant + H * log(k)
```

The study plots the fitted slope `H`, smoothed with a short EMA. Its default
interpretation deliberately uses a neutral zone:

- Above 0.60: persistent behavior; trend-following ideas may deserve preference.
- Below 0.40: anti-persistent behavior; mean-reversion ideas may deserve preference.
- Between 0.40 and 0.60: inconclusive.

## What it does not say

- It does not predict direction. A high value can accompany a persistent decline.
- It does not prove that the next breakout will work or fail.
- Exactly 0.5 does not mean "no edge"; it only describes the fitted scaling law.
- Values depend on the estimator, sampling interval, window, and smoothing. A value
  from a rescaled-range implementation is not directly interchangeable with this
  variance-scaling estimate.
- Sixty observations are enough for a responsive display, but not a precise
  long-memory estimate. Treat threshold crossings as noisy state estimates.

## Practical use

Use it as a filter on a strategy that already has defined entries, exits, and risk
controls. Compare the strategy out of sample in persistent, anti-persistent, and
neutral buckets. Keep the filter only if it improves results after turnover and
without repeatedly tuning the thresholds.

The default uses completed bars, so its daily reading does not drift with the current
unfinished candle. Disable `useCompletedBars` to include the live bar. The calculation
needs at least `length + 17` chart bars with the default enabled. `showRawEstimate`
exposes the unsmoothed estimate for diagnosis.
