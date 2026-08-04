---
name: team-ownership
description: >-
  Create two test teams in Port, assign them ownership of the `service`
  catalog entities, and add the current user to one of the teams. Use when
  the user asks about Port teams, ownership, RBAC by team, or wants to see
  how entities get owned by a team.
---

# Team Ownership

Creates two test teams (`platform-team`, `checkout-team`), assigns each
`service` entity to one of them as its owning team, and adds the current
user to `platform-team` — showing Port's built-in team-ownership model
(distinct from a regular blueprint relation).

This skill is tool-agnostic — use whatever Port API/MCP access you have,
whether invoked from Claude, Cursor, or Port AI. Teams and users are
built-in Port concepts, not custom blueprints, but they're managed the same
way as any other entity: teams live on blueprint `_team`, users on `_user`,
and ownership is the special top-level `team` field every entity has
(an array of team identifiers) — not a custom relation.

## Steps

1. **Ensure the `service` blueprint and its entities exist.** If not, run
   [catalog-foundations](../catalog-foundations/SKILL.md) first.
2. **Create the two teams.** Call `upsert_entity` with
   `blueprintIdentifier: "_team"` once per entry in
   [assets/teams.json](assets/teams.json):
   - `platform-team` — owns shared platform services.
   - `checkout-team` — owns checkout and billing.
3. **Assign service ownership.** For each service in
   [assets/service-ownership.json](assets/service-ownership.json), call
   `upsert_entity` on blueprint `service` setting the entity's top-level
   `team` field to `["<team-identifier>"]` (do not touch `properties` —
   `team` is a sibling field on the entity, same level as `identifier` and
   `properties`):
   - `checkout-team`: `checkout-service`, `billing-service`
   - `platform-team`: `auth-service`, `notifications-service`,
     `search-service`, `inventory-service`
4. **Add the current user to a team.** Find the current user's `_user`
   entity (its identifier is the user's email — look up the connected
   user's email via whatever "who am I" tool your environment exposes, e.g.
   `describe_user_details`, or ask the user for their Port email if none is
   available). Call `upsert_entity` on blueprint `_user` for that
   identifier, setting the top-level `team` field to `["platform-team"]`.

## Done when

Both teams exist under `_team`, every `service` entity shows an owning team
matching the mapping above, and the current user's `_user` entity lists
`platform-team`.

## Manual fallback (Port UI)

Settings → Teams → New Team → create `platform-team` and `checkout-team`
using the descriptions in [assets/teams.json](assets/teams.json). Catalog →
`service` → open each entity → set its Team field per
[assets/service-ownership.json](assets/service-ownership.json). Settings →
Members → find your user → add to `platform-team`.
