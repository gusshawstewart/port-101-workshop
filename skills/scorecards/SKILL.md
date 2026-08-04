---
name: scorecards
description: >-
  Create a "Production Readiness" scorecard on the `service` blueprint with
  Bronze/Silver/Gold rules. Use when the user asks about Port scorecards,
  maturity levels, readiness checks, or scoring/grading catalog entities.
---

# Scorecards

Creates a **Production Readiness** scorecard on the `service` blueprint,
showing how Port grades catalog entities against rules instead of relying on
spreadsheets or tribal knowledge.

This skill is tool-agnostic — use whatever Port API/MCP access you have
(`upsert_scorecard`), whether invoked from Claude, Cursor, or Port AI. Via
raw REST, the equivalent call is
`PUT /v1/blueprints/service/scorecards/production_readiness`.

## Steps

1. **Ensure the `service` blueprint exists.** If not, run the
   [catalog-foundations](../catalog-foundations/SKILL.md) skill first (or at
   minimum upsert a blueprint identified `service` with at least `tier`,
   `has_readme`, `on_call`, and `lifecycle` properties — this skill does not
   hard-depend on the other one, but the rules below reference those
   property identifiers).
2. **Create the scorecard.** Call `upsert_scorecard` with the payload in
   [assets/production-readiness-scorecard.json](assets/production-readiness-scorecard.json):
   - **Bronze** — `tier` is set.
   - **Silver** — has a README and an on-call owner.
   - **Gold** — Silver, plus `lifecycle` is `Production`.

## Done when

The `production_readiness` scorecard exists on `service`, and services in
the catalog show a visible spread of levels (not all Gold, not all Basic).

## Manual fallback (Port UI)

Catalog → `service` blueprint → Scorecards tab → New scorecard → recreate
the rules from
[assets/production-readiness-scorecard.json](assets/production-readiness-scorecard.json).
