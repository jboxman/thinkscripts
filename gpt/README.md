ChatGPT derived studies that seem worth keeping.

- `gpt_nyse_advance_decline_10dma.thinkscript`,
  `gpt_nyse_up_down_volume_10dma.thinkscript`,
  `gpt_nyse_net_volume_10dma.thinkscript`, and
  `gpt_nyse_high_low_5dma.thinkscript` reproduce the four NYSE breadth panes in
  the reference StockCharts layout. Add them separately to a daily chart.
- `wip/gpt_spx_weis_downswings.thinkscript` reuses the Weis ZigZag swing state and
  clouds peak-to-price drawdown area after a downswing reaches 5%.
- `gpt_hurst_regime.thinkscript` estimates rolling persistence from the
  variance-scaling slope of multi-horizon log returns and classifies only values
  outside a configurable neutral band.
- `gpt_lower_pivot_poe_bars.thinkscript` marks compressed volume-spike,
  no-supply/no-demand, and selling-exhaustion bars near a rolling lower
  extreme, plus a subsequent bullish ease-of-movement confirmation.
- `gpt_swing_vcp.thinkscript` builds a causal VCP approximation from confirmed
  swing highs/lows, progressively smaller higher-low contractions, volume/ATR
  dry-up, and a volume-confirmed breakout through the stored swing-high pivot.
