# SNACS MCP Server

Connect your AI assistant to live SEC dilution forensics, market data, and news for U.S. equities updated in real-time.

`https://mcp.snacs.trade` - remote MCP server, HTTP transport, no local install.

## What you can ask
- "Pull the full dilution chain state for TICKER - active shelfs, ATMs, warrants, converts."
- "What was this company's cash, float, and active facilities on 2024-12-31?" (point-in-time, no lookahead bias)
- "Screen for stocks under $5 with an active ATM and less than 6 months of runway."
- "Summarize today's 8-Ks and offering-related news for my watchlist."

## Setup

You need an API key: [snacs.trade/api](https://snacs.trade/api) -> subscribe -> account settings -> API keys (`snacs_sk_live_...`).

The server accepts the key two ways: `Authorization: Bearer snacs_sk_...` (shown below) or a plain `X-API-Key: snacs_sk_...` header (useful for gateways and directories that inject a single key header).

### Claude Desktop
Settings -> Connectors -> Add custom connector:
- URL: `https://mcp.snacs.trade`
- Header: `Authorization: Bearer snacs_sk_live_...`

### Claude Code
```bash
claude mcp add --transport http snacs https://mcp.snacs.trade \
  --header "Authorization: Bearer snacs_sk_live_..."
```

### Cursor
`.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "snacs": {
      "url": "https://mcp.snacs.trade",
      "headers": { "Authorization": "Bearer snacs_sk_live_..." }
    }
  }
}
```

### VS Code (Copilot)
`.vscode/mcp.json`:
```json
{
  "servers": {
    "snacs": {
      "type": "http",
      "url": "https://mcp.snacs.trade",
      "headers": { "Authorization": "Bearer snacs_sk_live_..." }
    }
  }
}
```

## Tools (14)
Pulled from the live server's `tools/list` (Aug 11 2026, server v3.4.2):

| Tool | What it does |
|---|---|
| `snacs_coverage_get` | Coverage discovery: universe-wide counts per data dimension, or one ticker's coverage. |
| `snacs_coverage_list` | List covered tickers for a dimension ('forensic-dilution' or 'market-data'), keyset-paginated. |
| `snacs_dilution_get` | Forensic dilution snapshot for a ticker: share counts, facility capacity, runway, and dilution risk state extracted from SEC filings. |
| `snacs_dilution_asof_get` | Point-in-time forensic state as of a date (YYYY-MM-DD): the share checkpoint effective on/before it, facilities in force, events up to it. |
| `snacs_facilities_get` | Dilution facilities (shelf/ATM/warrants/convertibles): capacity, usage, remaining, prices, status. |
| `snacs_checkpoints_get` | Authoritative point-in-time share-count checkpoints from SEC filings, newest first. |
| `snacs_events_get` | Corporate events (splits, offerings, redomestications), each substantiated by a verbatim SEC quote. |
| `snacs_screener_list` | Dilution screener over all covered tickers, keyset-paginated. |
| `snacs_news_get` | Recent news for a ticker, newest first. |
| `snacs_sec_filings_get` | Recent SEC filings for a ticker, optionally filtered by form type. |
| `snacs_fundamentals_get` | Latest fundamentals: float, market cap, shares outstanding, splits, cash runway/burn, short interest and short volume. |
| `snacs_quote_get` | Composite quote: live scanner snapshot during market hours, last-session snapshot off-hours, else the latest daily bar. |
| `snacs_bars_get` | Daily OHLCV bars, newest first, optionally bounded by date range. |
| `snacs_snapshot_get` | Live scanner snapshot: a single ticker's row, or the top of the board. |

## Pricing and limits
Included in every SNACS Data API tier within your rate cap. Edge $99/mo (50 req/min), Alpha $199/mo (1,000 req/min). Details: [snacs.trade/api](https://snacs.trade/api).

## Links
[API docs](https://data.snacs.trade/docs) · [OpenAPI spec](https://data.snacs.trade/v1/openapi.json) · [SNACS](https://snacs.trade)
