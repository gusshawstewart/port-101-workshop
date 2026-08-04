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
by a linear, three-node automation (create entity → trigger an Azure
DevOps Pipeline via Port's native
[Azure DevOps integration action](https://docs.port.io/workflows/build-workflows/action-nodes/integration-actions/azure-devops/),
as the stubbed provisioning "backend"). No branching, no AI — this is the
simplest possible trigger-to-automation example in Port.

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
   - `trigger_azure_pipeline` — `INTEGRATION_ACTION` node using Port's
     built-in Azure DevOps integration
     (`integrationProvider: "azure-devops"`,
     `integrationInvocationType: "trigger_pipeline"`), standing in for
     whatever *really* provisions a new service (repo, CI, infra). It
     uses `deferIntegrationInstallation: true`, so this workflow saves and
     runs even without a real Azure DevOps integration connected — the
     pipeline-trigger step just sits pending in the workflow's setup
     checklist until one is installed. **This is the stubbed/illustrative
     part of the demo**; `project` and `pipelineId` are placeholder values.
3. **(Optional) point it at a real pipeline** by installing the
   [Azure DevOps integration](https://docs.port.io/workflows/build-workflows/action-nodes/integration-actions/azure-devops/)
   (Single Account mode) from the workflow's setup checklist, then editing
   `project` / `pipelineId` in `trigger_azure_pipeline` to match a real
   Azure DevOps project and pipeline. Until then the run still succeeds
   end-to-end; only the pipeline-trigger node stays pending.

## Done when

The "Scaffold New Service" self-service action appears, and submitting it
creates a new `service` entity in the catalog.

## Manual fallback (Port UI)

Automations/Workflows → New Workflow → import
[assets/workflow.json](assets/workflow.json). Its `SELF_SERVE_TRIGGER` node
automatically makes it show up under Self-Service too.
