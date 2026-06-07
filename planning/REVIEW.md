# Review 2026-06-05 22:33

## Status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	planning/REVIEW.md

no changes added to commit (use "git add" and/or "git commit -a")

## Diff
git : warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
At line:1 char:41
+ $s = git status 2>&1 | Out-String; $d = git diff HEAD 2>&1 | Out-Stri ...
+                                         ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (warning: in the... Git touches it:String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
 
diff --git a/README.md b/README.md
index 6e90a5c..31317a9 100644
--- a/README.md
+++ b/README.md
@@ -2,19 +2,31 @@
 
 A Bloomberg-inspired trading terminal with live streaming market data and an AI assistant that can analyze your portfolio and execute trades via natural language.
 
-## Quick Start
+## Status
 
-```bash
-# Copy and configure environment variables
-cp .env.example .env
+| Component | Status |
+|---|---|
+| Market data subsystem (simulator + Polygon.io client, SSE stream) | Complete |
+| Backend API routes (portfolio, watchlist, chat, health) | Not started |
+| Database layer (SQLite, schema, seed data) | Not started |
+| LLM integration (chat, trade execution) | Not started |
+| Frontend (Next.js, charts, heatmap, AI chat panel) | Not started |
+| Docker build & deployment scripts | Not started |
+| E2E tests (Playwright) | Not started |
 
-# Start the app (single Docker container)
-docker run -v finally-data:/app/db -p 8000:8000 --env-file .env finally
-```
+## What's Built
 
-Open [http://localhost:8000](http://localhost:8000). No login required ΓÇö you start with $10,000 in virtual cash.
+The `backend/app/market/` package provides a complete market data layer:
 
-## Features
+- **GBM simulator** ΓÇö geometric Brownian motion with correlated sector moves and random shock events
+- **Polygon.io REST client** ΓÇö polls real market data when `MASSIVE_API_KEY` is set
+- **Shared interface** ΓÇö both sources implement the same ABC; downstream code is source-agnostic
+- **Thread-safe price cache** ΓÇö single point of truth for producers and consumers
+- **SSE stream endpoint** ΓÇö version-based change detection, `/api/stream/prices`
+
+73 tests passing, 84% coverage. Demo: `cd backend && uv run market_data_demo.py`
+
+## Planned Features
 
 - Live-streaming prices with green/red flash animations and sparkline mini-charts
 - Buy/sell via market orders ΓÇö instant fill, no confirmation dialog
@@ -40,16 +52,6 @@ Single container, port 8000:
 - **Market data**: GBM simulator by default; Polygon.io REST polling if `MASSIVE_API_KEY` is set
 - **LLM**: LiteLLM ΓåÆ OpenRouter (Cerebras inference) with structured JSON outputs
 
-## Development
-
-```bash
-# Backend
-cd backend && uv sync --extra dev && uv run pytest -v
-
-# Frontend
-cd frontend && npm install && npm run dev
-```
-
 ## Simulator Tickers
 
 AAPL, GOOGL, MSFT, AMZN, TSLA, NVDA, META, JPM, V, NFLX, AMD, INTC, PYPL, UBER, LYFT, COIN, SNAP, SPOT, PLTR, RBLX

