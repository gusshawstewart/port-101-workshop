---
name: catalog-foundations
description: >-
  Build the Port software catalog foundation: a `service` blueprint plus six
  mock service entities. Use when the user asks to set up a Port catalog,
  create a service blueprint, load sample/mock data, or start a Port POC from
  scratch.
---

# Catalog Foundations

Creates the `service` blueprint — the core of a Port software catalog — and
loads six mock services so the catalog isn't empty for a demo or POC.

This skill is tool-agnostic: it assumes you have access to Port's API or MCP
tools (`upsert_blueprint`, `upsert_entity`), whether you are Claude, Cursor,
or Port AI itself. If you only have the raw Port REST API, use the
equivalent `PUT /v1/blueprints/{identifier}` and
`PUT /v1/blueprints/{identifier}/entities/{identifier}?upsert=true` calls
with the same payloads.

## Steps

1. **Create the blueprint.** Call `upsert_blueprint` with the payload in
   [assets/service-blueprint.json](assets/service-blueprint.json). It defines
   `language`, `lifecycle`, `tier`, `on_call`, `has_readme`, and `repo_url` —
   enough spread of properties to make scorecards and workflows meaningful
   later.
2. **Load sample entities.** Call `upsert_entity` once per entity in
   [assets/sample-services.json](assets/sample-services.json) (6 services:
   `checkout-service`, `auth-service`, `notifications-service`,
   `billing-service`, `search-service`, `inventory-service`). Note the
   deliberate gaps — `billing-service`, `search-service`, and
   `inventory-service` are missing `on_call` and `has_readme` so a maturity
   scorecard has something to flag.

## Done when

The `service` blueprint exists and all six service entities appear in the
catalog.

## Manual fallback (Port UI)

Builder → Blueprints → Import → paste
[assets/service-blueprint.json](assets/service-blueprint.json). Then
Catalog → `service` → bulk-import or add entities from
[assets/sample-services.json](assets/sample-services.json).
