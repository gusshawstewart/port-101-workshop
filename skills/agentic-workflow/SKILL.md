---
name: agentic-workflow
description: >-
  Create an AI-powered self-service workflow ("AI Service Health Check")
  that fetches a service's catalog data, generates a readiness summary with
  an LLM node, and writes it back to the entity. Use when the user asks for
  an agentic/AI workflow example, an AI node in Port workflows, or how Port
  workflows can call an LLM.
---

# Agentic Workflow

Creates an **"AI Service Health Check"** self-service action and workflow:
pick a service, the workflow fetches its catalog properties, an `AI` node
generates a short readiness summary + recommendations, and the result is
written back onto the entity (`ai_health_summary`,
`ai_health_checked_at`). This is a small, 4-node example of the same shape
as [basic-workflow](../basic-workflow/SKILL.md), but with a real AI
generation step in the middle.

This skill is tool-agnostic — use whatever Port API/MCP access you have
(`upsert_blueprint`, `upsert_action`, `upsert_workflow`), whether invoked
from Claude, Cursor, or Port AI.

## Steps

1. **Patch the `service` blueprint** to add the two properties the workflow
   writes back: `ai_health_summary` (markdown) and `ai_health_checked_at`
   (date-time). Call `upsert_blueprint` with
   [assets/service-blueprint-patch.json](assets/service-blueprint-patch.json)
   — it merges these two new properties into whatever `service` blueprint
   already exists (from [catalog-foundations](../catalog-foundations/SKILL.md)),
   or creates the blueprint from scratch if it's missing.
2. **Create the action.** Call `upsert_action` with
   [assets/action.json](assets/action.json) — a single input: pick a
   `service` entity.
3. **Create the workflow.** Call `upsert_workflow` with
   [assets/workflow.json](assets/workflow.json). It has 4 nodes:
   - `trigger` — entity picker.
   - `fetch_service` — `WEBHOOK` GET to `https://api.getport.io/v1/blueprints/service/entities/{id}`
     (EU tenants: use `api.eu.getport.io`). Port authenticates this call for
     you; no manual credentials needed.
   - `ai_health_check` — `AI` node; prompts on the service's properties,
     returns `summary` + `recommendations` as structured JSON.
   - `write_back_to_catalog` — `UPSERT_ENTITY` writes the summary back onto
     the same service.

## Done when

Running "AI Service Health Check" against any service (e.g.
`billing-service`, which is missing `on_call` and `has_readme`) populates
that entity's `ai_health_summary` property with a summary and
recommendations.

## Manual fallback (Port UI)

Builder → `service` blueprint → merge in the two new properties from
[assets/service-blueprint-patch.json](assets/service-blueprint-patch.json).
Self-Service → New Action → import
[assets/action.json](assets/action.json). Automations/Workflows → New
Workflow → import [assets/workflow.json](assets/workflow.json).
