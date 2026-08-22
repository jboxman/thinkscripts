ChatGPT derived studies that seem worth keeping.

- `gpt_spx_drawdown_frequency.thinkscript` counts loaded-chart SPX 1% down
  sessions and repainting, close-based correction swings, with distinct minor
  circles and major peak/valley arrows.
- `gpt_hurst_regime.thinkscript` estimates rolling persistence from the
  variance-scaling slope of multi-horizon log returns and classifies only values
  outside a configurable neutral band.
- `gpt_lower_pivot_poe_bars.thinkscript` marks compressed volume-spike,
  no-supply/no-demand, and selling-exhaustion bars near a rolling lower
  extreme, plus a subsequent bullish ease-of-movement confirmation.
- `gpt_swing_vcp.thinkscript` builds a causal VCP approximation from confirmed
  swing highs/lows, progressively smaller higher-low contractions, volume/ATR
  dry-up, and a volume-confirmed breakout through the stored swing-high pivot.
