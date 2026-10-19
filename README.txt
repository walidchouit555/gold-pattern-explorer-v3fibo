Gold Pattern Explorer — Volatility Breakout Strategy v2

Included files
- app.py: separate v2 app. The original data-loading logic and Historical Matching section are retained.
- requirements.txt: required Python packages.

Deploy safely
1. Keep your current working repository unchanged as a backup.
2. Create a NEW GitHub repository for v2.
3. Upload the app.py and requirements.txt from this ZIP to the repository root.
4. Deploy the new repository on Streamlit Community Cloud and choose app.py.
5. First compare the automatic data range and Historical Matching results with your current version. Do not delete the old app.

Data and Historical Matching preserved
- Automatic XAU/USD hourly history fetched from the existing XAUS endpoint.
- CSV upload remains available as a fallback.
- The existing Historical Matching section is retained; only the strategy/backtest section has been replaced.

Strategy rules in v2
- Signal: current hourly candle High-Low must be strictly greater than the maximum range of the preceding 24 candles by default. The reference window can be adjusted in the app.
- Direction: a candle closing at or above its open creates a Buy signal; a red candle creates a Sell signal.
- Entry: 23.6% retracement of the signal candle range.
- Initial stop: opposite extreme of the signal candle.
- Final target: 161.8% extension beyond the signal candle's far end.
- On reaching 127.2% extension, move the stop to entry (break-even) for subsequent candles.
- On reaching 141.0% extension, move the stop to the 127.2% extension for subsequent candles.
- If price first reaches an adverse move equal to 68% of the original entry-to-stop distance, then a later candle returning to entry closes the trade at break-even. The adverse threshold and return cannot trigger this special exit in the same candle.
- Maximum waiting candles after signal: 0 means no time limit. Any newer signal replaces an older pending signal.
- Each signal is used for at most one entry. If price crosses the original stop before entry, the pending setup is invalidated.
- Only one position can be open at a time. By default, the newest signal during an open trade is kept pending until the position closes; an optional setting lets you ignore signals while a trade is open.
- No entry is allowed on the signal candle. If a trade closes inside an hourly candle, a pending setup is not allowed to enter until the next candle.
- For conservative handling of hourly OHLC candles, an active stop is checked before the final target if both appear touched in the same candle. New trailing stops take effect from the next candle because intrabar price order is unknown.

Important limitations
- The app's historical backtest is not a guarantee of future performance or a recommendation to trade.
- Trading costs (spread, commissions and slippage) are not included.
- Hourly OHLC data cannot reveal the precise order of price moves inside a candle; the backtest uses conservative assumptions for ambiguous cases.
- Open positions at the end of the dataset are marked to market in the trade table but excluded from closed-trade performance statistics.
