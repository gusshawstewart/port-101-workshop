---
name: basic-workflow
description: >-
  Create a simple, linear self-service action and workflow ("Scaffold New
  Service") with no branching or AI. Use when the user asks for a basic Port
  workflow example, a self-service action, or wants to see how a form
  triggers automation in Port.
---

# Basic Workflow

Creates a **"Scaffold New Service"** self-service action and its matching
workflow: a form triggers a linear, three-node automation (create entity →
notify Slack). No branching, no AI — this is the simplest possible
trigger-to-automation example in Port.

This skill is tool-agnostic — use whatever Port API/MCP access you have
(`upsert_action`, `upsert_workflow`), whether invoked from Claude, Cursor, or
Port AI.

## Steps

1. **Ensure the `service` blueprint exists.** If not, run the
   [catalog-foundations](../catalog-foundations/SKILL.md) skill first, or at
   minimum confirm a blueprint identified `service` exists with `language`,
   `lifecycle`, `tier`, `on_call`, `has_readme`, `repo_url` properties.
2. **Create the action (the form).** Call `upsert_action` with
   [assets/action.json](assets/action.json).
3. **Create the workflow (the automation).** Call `upsert_workflow` with
   [assets/workflow.json](assets/workflow.json). It has 3 nodes:
   - `trigger` — the same form as the action.
   - `create_service_entity` — `UPSERT_ENTITY` on blueprint `service`.
   - `notify_slack` — `WEBHOOK` to Slack (optional; fails silently
     (`onFailure: continue`) if `SLACK_BOT_TOKEN` / `SLACK_PLATFORM_CHANNEL`
     secrets aren't set).
4. **(Optional) add Slack secrets** on the workflow: `SLACK_BOT_TOKEN`,
   `SLACK_PLATFORM_CHANNEL`. Without them the run still succeeds; only the
   notification step fails.

## Done when

The "Scaffold New Service" self-service action appears, and submitting it
creates a new `service` entity in the catalog.

## Manual fallback (Port UI)

Self-Service → New Action → import [assets/action.json](assets/action.json).
Automations/Workflows → New Workflow → import
[assets/workflow.json](assets/workflow.json).
