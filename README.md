# Comparable Company Analysis Tool

A live, interactive web app that derives an implied valuation range for any company by benchmarking it against a peer group's trading multiples — the same "comps" workflow used in equity research and investment banking, automated.

**Live app:** https://comps-analysis-tool.streamlit.app/
*(URL kept from the original deployment — see note below)*

## What it does

1. Enter a target company ticker + a peer group (any Yahoo Finance ticker — US, NSE-listed Indian stocks with `.NS`, etc.)
2. Pulls live trading multiples: **EV/Revenue, EV/EBITDA, P/E, P/B, PEG**
3. Computes peer-group statistics (median, quartiles) — the target is excluded from its own benchmark
4. Applies those peer multiples back to the target's own financials to produce an **implied share-price range per methodology**
5. Visualizes the result as an industry-standard **football field chart**, plus supporting peer comparison and growth-adjusted valuation charts

## Notable engineering details

- **Currency normalization**: Yahoo Finance sometimes reports a company's financial-statement data (revenue, EBITDA) in a different currency than its quoted share price. The app detects this via Yahoo's `financialCurrency` field and auto-converts using a live FX rate before computing any multiple — without this, multiples for some Indian large-caps came out ~80x inflated.
- **Sanity-bound guard**: any computed multiple outside a plausible real-world range is flagged and excluded from peer statistics rather than silently corrupting the analysis.
- **Caching**: per-ticker data is cached for 15 minutes to avoid Yahoo Finance rate-limiting under repeated use.
- **Graceful degradation**: invalid tickers, missing fundamentals, and failed fetches are surfaced to the user with clear messages rather than crashing the app.

## Tech stack

`Python` · `Streamlit` · `yfinance` · `pandas` · `numpy` · `Plotly`

## Run locally

```bash
git clone https://github.com/aahil0/comps-analysis-tool.git
cd comps-analysis-tool
pip install -r requirements.txt
streamlit run app.py
```

## Deploy your own copy

Free on [Streamlit Community Cloud](https://share.streamlit.io) — connect this repo, set the main file to `app.py`, deploy.

## Related project

This app is a companion to my [Nifty 50 Multi-Factor Equity Screener](https://github.com/aahil0/nifty50-multi-factor-screener) — that project ranks stocks cross-sectionally on Value/Momentum/Quality/Low-Volatility factors; this one does relative (comps-based) valuation for a single target company. Different quant technique, same domain.

## Resume bullet

> Built and deployed a Comparable Company Analysis web app (Python, Streamlit, yfinance, Plotly) that derives implied equity valuation ranges from peer trading multiples, including automated currency-mismatch detection and correction in live financial data.

## License

MIT
