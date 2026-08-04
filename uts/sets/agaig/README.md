# AGAIG ThinkOrSwim Study Set

## Status

This is an archival collection of ThinkOrSwim studies assembled from multiple
authors and sources. It is retained for posterity and as a reference for
ThinkScript techniques. It is not maintained as a trading system, and the
individual or combined studies are not recommended for live use without
independent validation.

The files were cleaned conservatively to remove calculations that could not
affect a plot, label, alert, scan, assertion, or price color. Attribution,
origin, version, and modification-history comments were retained where they
were present in the source.

## What the collection is ultimately trying to do

Taken together, these studies try to turn an intraday price chart into a
decision-support dashboard. They repeatedly ask six broad questions:

1. **Where is price?**  
   Is it above or below VWAP, the opening range, a moving average, the current
   daily range, or a previously identified pivot?

2. **Which direction is price moving?**  
   Are moving averages aligned, is MACD positive, is a trend state bullish,
   or are smoothed candles pointing in the same direction?

3. **Is momentum turning?**  
   Has an oscillator, TTM histogram, CCI calculation, or short-term price
   comparison changed direction?

4. **Is the move receiving participation?**  
   Is volume unusually high, does the candle suggest buying or selling
   pressure, or are large technology stocks and index ETFs moving similarly?

5. **How volatile is the instrument?**  
   What are its daily, weekly, and monthly ATR values, and has price recently
   made a volatility-band excursion?

6. **Has price reached a possible reversal point?**  
   Has a ZigZag pivot, rolling high or low, or breakout from an extreme bar
   identified a potential turn?

The intended workflow appears to be: establish trend and location, wait for a
reversal or momentum signal, and then seek confirmation from volume or a
collection of additional indicators.

## The central limitation

Most of the apparent confirmation is not independent.

EMA alignment, MACD, TMO, CCI, DMI, SuperTrend, TTM Trend, Heikin-Ashi, and
polynomial smoothing all transform substantially the same OHLC price history.
They respond at different speeds and use different normalization methods, but
they usually measure the same underlying factor: recent price direction.

Consequently, several green labels or votes can mean that one upward price
move was measured several ways—not that several independent sources of
information confirmed it. The large dashboard studies are particularly
susceptible to this false sense of diversification.

The ZigZag-derived studies are even more closely related. They share the same
`ZigZagHighLow` pivot engine and mainly differ in presentation or secondary
filters. Running several together adds little information. The most recent
ZigZag pivot can also move while a swing is forming, so historical arrows
should not be interpreted as signals that were necessarily available at that
historical bar.

## Indicator families

| Family | Studies | Intended contribution |
| --- | --- | --- |
| Session location | ORB, VWAP, Murrey range levels, Price Pointer | Show price relative to intraday reference points |
| Trend and momentum | 20 EMA, polynomial MA, TMO, ST Turn Signals | Describe the direction or rate of change of price |
| Composite confirmation | Confirming Six, Dashboard Traffic Light | Combine several mostly price-derived trend votes |
| Swing reversal | Trend Arrows, AsGoodAsItGets Indicator, squared histogram, 3Trend, Enhanced Reversal, Reversal Pivot Lines | Mark or project potential turning points |
| Participation | Trend Volume Strength, Magnificent Seven Trend | Measure unusual volume or cross-symbol pressure |
| Volatility and setup | ATR label, ST Momentum Reversal | Describe range or require a volatility excursion before a breakout |

Of these families, session structure, volatility, current-symbol volume, and
cross-symbol data are the most meaningfully different from ordinary
price-trend transformations.

## Minimal combined study

[`shared_AGAIG_MinimalIntradayStack.thinkscript`](shared_AGAIG_MinimalIntradayStack.thinkscript)
was created to test a reduced version of the collection in one upper study. It
contains independently toggleable:

- 20 EMA
- VWAP
- 15-minute opening range
- daily, weekly, and monthly ATR label
- relative-volume spike dots
- ST Momentum Reversal arrows and hidden scan plots

This combination was chosen to represent trend, location, volatility,
participation, and one event-driven setup without loading several versions of
the same momentum calculation. It remains an experimental convenience, not a
validated strategy.

For a narrower visual review, 
[`shared_AGAIG_ORB_VWAP_RepaintingBubbles.thinkscript`](shared_AGAIG_ORB_VWAP_RepaintingBubbles.thinkscript)
combines only the opening range, VWAP, and the original ZigZag-derived
LONG/SHORT swing bubbles. The study includes independent display switches and
an on-chart warning that the newest ZigZag marker can move or disappear.

## Study index

| Study | Synopsis |
| --- | --- |
| [`shared_AGAIG_MinimalIntradayStack.thinkscript`](shared_AGAIG_MinimalIntradayStack.thinkscript) | Combines the reduced testing stack in one independently toggleable upper study |
| [`shared_AGAIG_ORB_VWAP_RepaintingBubbles.thinkscript`](shared_AGAIG_ORB_VWAP_RepaintingBubbles.thinkscript) | Bundles ORB, VWAP, and explicitly labeled repainting ZigZag LONG/SHORT swing markers |
| [`shared_AGAIGMurreyMathPivotsNoBubbles.thinkscript`](shared_AGAIGMurreyMathPivotsNoBubbles.thinkscript) | Plots the developing daily high, low, and 1/8 and 7/8 range fractions |
| [`shared_AGAIG_ORB_NoCloud_Lines_Labels.thinkscript`](shared_AGAIG_ORB_NoCloud_Lines_Labels.thinkscript) | Extends the initial opening-range high and low and labels price location |
| [`shared_AGAIG_TheConfirmingSixIndicators.thinkscript`](shared_AGAIG_TheConfirmingSixIndicators.thinkscript) | Displays six mostly trend-related labels and paints bars when five align |
| [`shared_AGAIG_TheDashboardTrafficLight.thinkscript`](shared_AGAIG_TheDashboardTrafficLight.thinkscript) | Converts twelve mostly price-derived conditions into a traffic-light vote |
| [`shared_AGAIG_TrendArrows.thinkscript`](shared_AGAIG_TrendArrows.thinkscript) | Displays ATR ZigZag swing arrows |
| [`shared_AGAIG_TrendVolumeStrength.thinkscript`](shared_AGAIG_TrendVolumeStrength.thinkscript) | Marks bars whose volume is sufficiently above its recent average |
| [`shared_AGAIG_VWAP_AUTO.thinkscript`](shared_AGAIG_VWAP_AUTO.thinkscript) | Plots a selectable daily, weekly, or monthly VWAP |
| [`shared_ATR_DWM_Label.thinkscript`](shared_ATR_DWM_Label.thinkscript) | Shows daily, weekly, and monthly ATR values |
| [`shared_AsGoodAsItGets_Indicator.thinkscript`](shared_AsGoodAsItGets_Indicator.thinkscript) | Combines ZigZag pivot markers with Kijun-related confirmation states |
| [`shared_AsGoodAsItGets_PricePointer.thinkscript`](shared_AsGoodAsItGets_PricePointer.thinkscript) | Extends the latest price into the chart expansion area |
| [`shared_AsGoodAsItGets_UpperSquaredHistogram1.thinkscript`](shared_AsGoodAsItGets_UpperSquaredHistogram1.thinkscript) | Displays ZigZag pivots as lower-pane histogram pulses |
| [`shared_AsGood_20_EMA_Line.thinkscript`](shared_AsGood_20_EMA_Line.thinkscript) | Plots a configurable exponential moving average |
| [`shared_AsGood_3TrendIndicator.thinkscript`](shared_AsGood_3TrendIndicator.thinkscript) | Combines rolling pivots and ZigZag arrows in three visual styles |
| [`shared_AsGood_EnhancedTrendReversalArrows1.thinkscript`](shared_AsGood_EnhancedTrendReversalArrows1.thinkscript) | Filters ZigZag reversal arrows by a custom VWAP state |
| [`shared_AsGood_ReversalPivotLines.thinkscript`](shared_AsGood_ReversalPivotLines.thinkscript) | Projects levels from ZigZag reversals using an EMA trend state |
| [`shared_AsGood_TrueMomentumOscillator.thinkscript`](shared_AsGood_TrueMomentumOscillator.thinkscript) | Smooths comparisons between the current close and previous opens |
| [`shared_MagnificentSeveTrend.thinkscript`](shared_MagnificentSeveTrend.thinkscript) | Estimates buying and selling pressure for the chart, Mag Seven, QQQ, and SPY |
| [`shared_PolynomialQuadraticMovingAverage.thinkscript`](shared_PolynomialQuadraticMovingAverage.thinkscript) | Fits a rolling quadratic curve to the average of close and VWAP |
| [`shared_ST_MomentumReversal.thinkscript`](shared_ST_MomentumReversal.thinkscript) | Looks for a volatility-band excursion followed by an extreme-bar breakout |
| [`shared_ST_TurnSignals.thinkscript`](shared_ST_TurnSignals.thinkscript) | Marks local turns in the TTM Squeeze histogram |

## Known reasons for caution

- Most studies have not been validated against out-of-sample data, realistic
  execution, slippage, or transaction costs.
- A large number of indicators does not imply diverse evidence when they all
  derive from the same price history.
- ZigZag studies can revise the latest swing and make historical charts look
  cleaner than the real-time experience.
- Secondary aggregations and cross-symbol studies depend on ThinkOrSwim data
  availability and chart timeframe.
- The opening-range studies are intended for intraday charts whose aggregation
  is no greater than the configured ORB aggregation.
- Several studies retain questionable design choices from their source,
  including forward plot offsets, unnormalized cross-symbol volume, and
  thresholds that do not adjust when components are disabled.
- None of the scripts defines complete position sizing, risk management,
  execution, or exit rules.

## Archival conclusion

The collection is useful as an example of how a discretionary chart can grow
into a large stack of correlated indicators. Its most durable lesson is not
that a particular combination should be traded, but that indicator diversity
should be judged by underlying information sources rather than names,
formulas, colors, or the number of agreeing signals.
