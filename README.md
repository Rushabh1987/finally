# FinAlly — AI Trading Workstation

A Bloomberg-inspired trading terminal with live streaming market data and an AI assistant that can analyze your portfolio and execute trades via natural language.

## Status

| Component | Status |
|---|---|
| Market data subsystem (simulator + Polygon.io client, SSE stream) | Complete |
| Backend API routes (portfolio, watchlist, chat, health) | Not started |
| Database layer (SQLite, schema, seed data) | Not started |
| LLM integration (chat, trade execution) | Not started |
| Frontend (Next.js, charts, heatmap, AI chat panel) | Not started |
| Docker build & deployment scripts | Not started |
| E2E tests (Playwright) | Not started |

## What's Built

The `backend/app/market/` package provides a complete market data layer:

- **GBM simulator** — geometric Brownian motion with correlated sector moves and random shock events
- **Polygon.io REST client** — polls real market data when `MASSIVE_API_KEY` is set
- **Shared interface** — both sources implement the same ABC; downstream code is source-agnostic
- **Thread-safe price cache** — single point of truth for producers and consumers
- **SSE stream endpoint** — version-based change detection, `/api/stream/prices`

73 tests passing, 84% coverage. Demo: `cd backend && uv run market_data_demo.py`

## Planned Features

- Live-streaming prices with green/red flash animations and sparkline mini-charts
- Buy/sell via market orders — instant fill, no confirmation dialog
- Portfolio heatmap (treemap) sized by weight, colored by P&L
- P&L chart tracking total portfolio value over time
- AI chat assistant that analyzes positions and executes trades on your behalf

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `OPENROUTER_API_KEY` | Yes | OpenRouter key for LLM chat |
| `MASSIVE_API_KEY` | No | Polygon.io key for real market data; omit to use the built-in simulator |
| `LLM_MOCK` | No | Set `true` for deterministic mock responses (testing/CI) |

## Architecture

Single container, port 8000:

- **Frontend**: Next.js static export served by FastAPI
- **Backend**: FastAPI (Python/uv), SQLite
- **Real-time**: Server-Sent Events (`GET /api/stream/prices`)
- **Market data**: GBM simulator by default; Polygon.io REST polling if `MASSIVE_API_KEY` is set
- **LLM**: LiteLLM → OpenRouter (Cerebras inference) with structured JSON outputs

## Simulator Tickers

AAPL, GOOGL, MSFT, AMZN, TSLA, NVDA, META, JPM, V, NFLX, AMD, INTC, PYPL, UBER, LYFT, COIN, SNAP, SPOT, PLTR, RBLX
