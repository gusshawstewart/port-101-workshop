---
name: catalog-foundations
description: >-
  Build the Port software catalog foundation: a `service` blueprint related
  to `azure_repo` and `k8s_resource` blueprints, plus mock entities for all
  three. Use when the user asks to set up a Port catalog, create a service
  blueprint, model relations between services/repos/k8s resources, load
  sample/mock data, or start a Port POC from scratch.
---

# Catalog Foundations

Creates three related blueprints — `service`, `azure_repo`, and
`k8s_resource` — and loads mock entities for each, so the catalog isn't
empty for a demo or POC and shows a simple, realistic relation graph
(service → repo, service → k8s resources).

This skill is tool-agnostic: it assumes you have access to Port's API or MCP
tools (`upsert_blueprint`, `upsert_entity`), whether you are Claude, Cursor,
or Port AI itself. If you only have the raw Port REST API, use the
equivalent `PUT /v1/blueprints/{identifier}` and
`PUT /v1/blueprints/{identifier}/entities/{identifier}?upsert=true` calls
with the same payloads.

## Steps

1. **Create the `azure_repo` blueprint.** Call `upsert_blueprint` with
   [assets/azure-repo-blueprint.json](assets/azure-repo-blueprint.json) —
   `url`, `project`, `default_branch`.
2. **Create the `k8s_resource` blueprint.** Call `upsert_blueprint` with
   [assets/k8s-resource-blueprint.json](assets/k8s-resource-blueprint.json)
   — `kind` (Deployment/StatefulSet/CronJob), `namespace`, `cluster`,
   `replicas`.
3. **Create the `service` blueprint.** Call `upsert_blueprint` with
   [assets/service-blueprint.json](assets/service-blueprint.json). It
   defines `language`, `lifecycle`, `tier`, `on_call`, `has_readme`,
   `repo_url`, plus two relations: `azure_repo` (single) and
   `k8s_resources` (many) — enough spread of properties and relations to
   make scorecards and workflows meaningful later. Create this after steps
   1–2 so the relation targets already exist.
4. **Load `azure_repo` entities.** Call `upsert_entity` once per entity in
   [assets/sample-azure-repos.json](assets/sample-azure-repos.json) (one
   repo per service).
5. **Load `k8s_resource` entities.** Call `upsert_entity` once per entity in
   [assets/sample-k8s-resources.json](assets/sample-k8s-resources.json)
   (one workload per service).
6. **Load `service` entities with their relations.** Call `upsert_entity`
   once per entity in
   [assets/sample-services.json](assets/sample-services.json) (6 services:
   `checkout-service`, `auth-service`, `notifications-service`,
   `billing-service`, `search-service`, `inventory-service`), each
   including `relations.azure_repo` and `relations.k8s_resources`. Note the
   deliberate gaps — `billing-service`, `search-service`, and
   `inventory-service` are missing `on_call` and `has_readme` so a maturity
   scorecard has something to flag.

## Done when

All three blueprints exist (with `service` related to both `azure_repo` and
`k8s_resource`), and all entities appear in the catalog with their
relations populated.

## Manual fallback (Port UI)

Builder → Blueprints → Import → paste
[assets/azure-repo-blueprint.json](assets/azure-repo-blueprint.json), then
[assets/k8s-resource-blueprint.json](assets/k8s-resource-blueprint.json),
then [assets/service-blueprint.json](assets/service-blueprint.json). Then
import entities from
[assets/sample-azure-repos.json](assets/sample-azure-repos.json),
[assets/sample-k8s-resources.json](assets/sample-k8s-resources.json), and
[assets/sample-services.json](assets/sample-services.json), in that order.
