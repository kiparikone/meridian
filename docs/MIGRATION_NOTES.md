# Migration notes — why this repo exists, and the honest read on it

**Date:** 2026-09-28
**Trigger:** Kip, after the RLS/hybrid-architecture work: *"Seriously making me think all my eggs are in one basket. Any alternative to truly own what we make?"* → decision: start the migration, target a clean Phase 2 start 10/1.

## The actual question, restated

Meridian's real data — 62 scored stocks, 7 predictions (4 evaluated, 3 open), 4 closed Phase-1 positions, 6 scoring runs, 5 framework configs, 11 binary events — currently lives in three places that don't agree with each other: Base44 (richest, current source of truth), Supabase (older, partially superseded, RLS-locked), and Airtable (decisions and cost log, not test data). None of these are Kip's in the sense that matters: if Base44's Builder plan lapses (already flagged as expiring ~March 2027) or Anthropic changes what MCP tools are available, the data doesn't leave with him unless someone pulls it out first.

This repo is that pull. Plain JSON, git-tracked, under Kip's own GitHub account. No vendor can revoke it, and git gives free version history Base44 never offered.

## What this is *not*

Applying Kip's own project governance rule directly: **"closer to true delegation," "this is different this time," and similar framing have recurred multiple times (Apr/Jul/Sep 2026) with infrastructure built and mostly abandoned within weeks. What separated what survived from what died wasn't a new tool or audience — it was whether Kip had independent reason to keep feeding it regardless of external response."**

Applying that test here, honestly, before endorsing this as different:

- This is not new infrastructure to feed. It's the opposite — a static export of data that already exists, that requires zero ongoing care to remain useful (unlike a Base44 app, a Lovable build, or a Supabase project, none of which can go quiet for a week without a pause/timeout/staleness problem). A JSON file doesn't need "feeding."
- The independent reason to keep this alive isn't audience or growth — it's the same reason Kip keeps a git repo for actual code: the data represents 5 months of real, hard-won test history (the Phase 1 closure numbers, the framework's honest self-scored 50% calibration bar, the corrected PredictionLog fields) that would be expensive to reconstruct and embarrassing to lose. That's a durable reason independent of whether anyone else ever looks at this repo.
- **Where this motion COULD fail the same way prior ones did:** if it grows beyond a data export — if it starts accumulating a Python scoring engine, a dashboard, deploy tooling — it becomes exactly the kind of infrastructure the governance rule warns about, one more thing that needs feeding. **Deliberately not built here:** no scoring logic, no dashboard, no automation. Just the data and a written record of where it came from. If Kip wants a viewing layer, that's a separate, later decision — an Artifact reading these JSON files directly, not more infrastructure in this repo.

## Current state (2026-09-28)

Done, this session, without waiting on anything:
- Repo scaffolded locally (`git init`, real commit history starts here)
- All 6 live AlphaStrata entity tables exported to `data/*.json` — 95 real records total, verified by direct `query_entities` calls this session, not copied from memory or an earlier summary
- Provenance and known-gaps documented (`docs/PROVENANCE.md`)

**Blocked, needs Kip:** this session has **zero GitHub repositories currently attached** (`list_repos` returned empty), and there is no tool here that can create a new repo on GitHub — only attach an existing one. Two ways to unblock, Kip's call:
1. Kip creates an empty repo on GitHub under his own account (e.g. `kiparikone/meridian`) — 30 seconds, and arguably the more correct move given the whole point is *his* ownership, not another AI-provisioned resource — then tells me the name and I attach it (push access) and push what's already committed locally.
2. Kip points me at an existing repo he already has that this should live in/alongside.

Either way, nothing here is lost in the meantime — it's a real local git repo with a real commit, sitting in the session's working directory, ready to push the moment there's a remote.

## Separate, related question Kip raised: do we even need Base44?

Kip's own framing: *"If the public.com agent turns out to be able to handle the 'CIO brief' and listen to signals, everything else in AlphaStrata to me can be setup locally as an artifact or built using Claude Code if needed. Am I wrong?"*

He's not wrong, and this export is a step toward finding out either way without a hard commitment now:
- If the answer turns out to be "yes, drop Base44," this repo is already the landing spot — no data gets stranded.
- If the answer is "keep Base44 through March 2027 since it's already paid," this repo is still worth having as the portable backup, and costs nothing extra to maintain (static files, no subscription).
- The CIO Daily Brief page built in Base44 (Aug 19, verification pass pending) is the one piece of *logic*, not just data, currently living only in Base44. If Base44 is dropped, that page's logic (staleness check off ScoringRun.next_run_date, signal surfacing, guardrails) would need to be rebuilt somewhere — likely as a small Python script reading these JSON files, run on demand or via a scheduled task, output to an Artifact. Not urgent; flagged for the Base44-necessity decision, not decided here.

This is a decision for Kip, not a recommendation — organizing the tradeoff, not making the call.
