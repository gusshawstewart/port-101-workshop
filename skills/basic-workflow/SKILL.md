---
name: basic-workflow
description: >-
  Create a simple, linear self-service action and workflow ("Scaffold New
  Service") with no branching or AI. Use when the user asks for a basic Port
  workflow example, a self-service action, or wants to see how a form
  triggers automation in Port.
---

# Basic Workflow

Creates a **"Scaffold New Service"** workflow: its first node is a
self-service trigger (the form attendees see under Self-Service), followed
by a linear, three-node automation (create entity → notify Slack). No
branching, no AI — this is the simplest possible trigger-to-automation
example in Port.

This skill is tool-agnostic — use whatever Port API/MCP access you have
(`upsert_workflow`), whether invoked from Claude, Cursor, or Port AI. The
workflow's first node (`type: SELF_SERVE_TRIGGER`) *is* the self-service
form — you don't need a separate action object.

## Steps

1. **Ensure the `service` blueprint exists.** If not, run the
   [catalog-foundations](../catalog-foundations/SKILL.md) skill first, or at
   minimum confirm a blueprint identified `service` exists with `language`,
   `lifecycle`, `tier`, `on_call`, `has_readme`, `repo_url` properties.
2. **Create the workflow.** Call `upsert_workflow` with
   [assets/workflow.json](assets/workflow.json). It has 3 nodes:
   - `trigger` — `SELF_SERVE_TRIGGER`, the form.
   - `create_service_entity` — `UPSERT_ENTITY` on blueprint `service`.
   - `notify_slack` — `WEBHOOK` to Slack (optional; fails silently
     (`onFailure: continue`) if `SLACK_BOT_TOKEN` / `SLACK_PLATFORM_CHANNEL`
     secrets aren't set).
3. **(Optional) add Slack secrets** on the workflow: `SLACK_BOT_TOKEN`,
   `SLACK_PLATFORM_CHANNEL`. Without them the run still succeeds; only the
   notification step fails.

## Done when

The "Scaffold New Service" self-service action appears, and submitting it
creates a new `service` entity in the catalog.

## Manual fallback (Port UI)

Automations/Workflows → New Workflow → import
[assets/workflow.json](assets/workflow.json). Its `SELF_SERVE_TRIGGER` node
automatically makes it show up under Self-Service too.
