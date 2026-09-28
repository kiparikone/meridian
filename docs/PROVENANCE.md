# Provenance

**Exported:** 2026-09-28, by K-laude (CFO lane) at Kip's request.
**Source:** Base44 app "AlphaStrata" — app id `69ca87fae153629260c39bb7`, owner `tokiplatform@gmail.com`.
**Method:** `mcp__Base44__query_entities` against each entity, full table scan (limit 500, all tables under 100 rows), written to flat JSON as-is — no transformation, no filtering, no re-scoring.

## What this snapshot is

This is a **point-in-time export**, not a live sync. AlphaStrata (Base44) remains the system of record until Kip decides otherwise — see `MIGRATION_NOTES.md` for the actual decision in front of him. Until then, this repo can drift from Base44 if either side gets written to independently. Treat any number in here older than the `updated_date` on the matching Base44 record as stale.

## What was NOT migrated

- **PortfolioPosition** entity — marked DEPRECATED in Base44 on 2026-08-19 (superseded by `Position`, which has the richer schema: `position_size_pct`, `exit_price`, `exit_reason`, `scored_stock_id`). No live data worth carrying over.
- **User** entity — Base44 auth/role table, not test data.
- Anything in Supabase project `havsawjlwkiijkwxvqxi` — that's a *separate* backend (the original, now largely superseded, scoring-engine attempt) with its own RLS-locked tables. Not in scope for this export. If Supabase is ever fully retired, its `portfolio_positions` rows (4, the same 4 Phase-1 ETFs, populated 8/19 with the version of the entry data known *at the time* — since corrected here with real order-history data on 9/28) should be checked for drift before deletion, not just abandoned.

## Known unreconciled gap (carried over, not fixed)

`scored_stocks.json` contains two populations under `framework: "AI/Robotics"` that do not share a lineage:
1. The **real, validated 12-ticker BOTZ-seeded universe** (NVDA, ISRG, CGNX, PEGA, DT, PATH, SYM, SOUN, UPST, AI, AVAV, PRCT) — tied to actual `ScoringRun` records, actual `PredictionLog` entries, and the framework's own self-scored calibration history. This is the framework Kip is actually testing.
2. A **pre-existing mega-cap set** (NVDA, MSFT, GOOGL, META, AMZN, TSM, AVGO, AMD, PLTR, INTC, ARKK, AI) with `run_id: null` — no linked ScoringRun, never evaluated, origin unknown (predates this CFO-lane engagement). Flagged during the 8/31 refresh, not investigated further at Kip's direction ("flagged, not fixed, this pass").

Both share the ticker NVDA and AI, at different composite scores, under the same framework label. Any consumer of this data (a dashboard, a brief generator) needs to filter on `run_id` to avoid mixing them.
