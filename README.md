# Meridian

Real-money AI-investment test data, exported so Kip owns it outright — not locked inside any single vendor (Base44, Supabase, or an AI session).

- **What this is:** a git-tracked snapshot of the Meridian/AlphaStrata test data — scoring runs, scored stocks, predictions (evaluated and open), the closed Phase 1 ETF positions, framework configs, and binary catalysts. Plain JSON, no build step, no dependencies.
- **What this is not:** a running app. No scoring engine, no dashboard, no automation lives here on purpose — see `docs/MIGRATION_NOTES.md` for why that's deliberate, not an oversight.
- **Source of truth today:** Base44 app "AlphaStrata" remains authoritative until Kip decides otherwise. This repo is the portable, ownable copy.

## Layout

```
data/
  scored_stocks.json      62 records — all 5 frameworks, incl. the real BOTZ-12 AI/Robotics universe + v1.1 refresh
  scoring_runs.json       6 records — 1 completed (v1.1, 8/31/26), 5 backfilled/overdue
  prediction_log.json     7 records — 4 evaluated (2 hit, 2 miss on direction; 3 of 4 outperformed SPY), 3 open (due 11/30/26)
  positions.json          4 records — the closed Phase 1 ETF positions (VUG/SCHD/XLP/SMH), real order-history data
  framework_config.json   5 records — current weights + real market-context notes per sector
  binary_events.json      11 records — dated catalysts (PDUFA dates, earnings, trial readouts)
  schemas.json            field reference for all of the above

docs/
  PROVENANCE.md           exactly where this data came from, what was excluded, and a known unreconciled gap
  MIGRATION_NOTES.md      why this repo exists, the honest read against Kip's own "before endorsing new motion" test, and current status
```

## Status

See `docs/MIGRATION_NOTES.md` — short version: data's exported and committed locally, blocked on Kip creating (or naming) a GitHub repo to push to.
