# Port 101 — a beginner's tour of Port

Four small, **independent** skills, each showing one Port capability, built
on a simple software-catalog domain. Aimed at people about to run their own
Port POC and want a quick, hands-on tour of the core building blocks:
**catalog → scorecards → basic workflow → agentic (AI) workflow.**

Unlike a guided multi-step workshop, these skills don't depend on each
other and don't need to be run in order — pick whichever feature you want
to see first.

## What's here

| Skill | Shows you | Files |
|---|---|---|
| [catalog-foundations](skills/catalog-foundations/SKILL.md) | Blueprints + mock entities | [skills/catalog-foundations/](skills/catalog-foundations/) |
| [scorecards](skills/scorecards/SKILL.md) | Grading entities with rules (Bronze/Silver/Gold) | [skills/scorecards/](skills/scorecards/) |
| [basic-workflow](skills/basic-workflow/SKILL.md) | A self-service form triggering a linear, no-AI automation | [skills/basic-workflow/](skills/basic-workflow/) |
| [agentic-workflow](skills/agentic-workflow/SKILL.md) | The same shape, but with an AI node generating real output | [skills/agentic-workflow/](skills/agentic-workflow/) |

Background on the fictional-but-simple catalog domain:
[company-context.md](company-context.md).

## Prerequisites

- A [Port account](https://app.getport.io/signup) (free tier works).
- One of:
  - **Claude or Cursor**, with Port MCP configured (`https://mcp.port.io/v1`
    or EU: `https://mcp.port-eu.io/v1`), **or**
  - **Port AI chat**, with the matching skill entity imported (see below).

## These are agnostic skills, not tool-specific

Each skill is a plain `SKILL.md` + `assets/` folder under [skills/](skills/)
— no `.cursor/` or `.claude/` prefix, no Cursor-only conventions. Use it
however your tool expects skills to be discovered:

- **Claude / Claude Code**: point it at, or copy/symlink, the relevant
  folder under `skills/` into wherever your Claude setup looks for skills
  (e.g. `.claude/skills/`), or just paste the `SKILL.md` contents into the
  chat and ask Claude to follow it.
- **Cursor**: same idea — copy/symlink into `.cursor/skills/`, or reference
  the skill by name/path in chat.
- **Port AI chat**: Port doesn't read files from this repo directly. Import
  the matching **Port skill entity** from [port-skills/](port-skills/)
  instead (one JSON file per skill, plus the shared `skill` blueprint) —
  then ask Port AI in plain language (e.g. *"scaffold a new service called
  reporting-service"* or *"run an AI health check on billing-service"*).

### Using a skill with Claude or Cursor

1. Open this repo (or just the relevant `skills/<name>/` folder) in your
   tool.
2. Ask the assistant to follow the skill, e.g. *"Follow
   skills/catalog-foundations/SKILL.md"*.
3. It reads the referenced `assets/*.json` payloads and calls the
   corresponding Port MCP tools (`upsert_blueprint`, `upsert_entity`,
   `upsert_scorecard`, `upsert_action`, `upsert_workflow`).

### Using a skill in Port AI chat

1. Import blueprint **`skill`** from
   [port-skills/skill-blueprint.json](port-skills/skill-blueprint.json)
   (skip if your org already has it).
2. Import the entity for whichever skill you want from
   [port-skills/](port-skills/) (e.g.
   [port-skills/basic-workflow.json](port-skills/basic-workflow.json)).
3. Ask Port AI to do the thing in plain language — it loads the matching
   skill entity and acts on it.

Note: the `port-skills/` entities for `basic-workflow` and
`agentic-workflow` only **trigger** the workflow — you still need to have
created the action/workflow itself first (via the `SKILL.md` in
[skills/basic-workflow/](skills/basic-workflow/) or
[skills/agentic-workflow/](skills/agentic-workflow/), run once from Claude
or Cursor with Port MCP).

## What you'll have at the end (if you run all four)

1. A `service` catalog with six services with varying maturity.
2. A "Production Readiness" scorecard showing a real spread of
   Bronze/Silver/Gold.
3. A working self-service form ("Scaffold New Service") that creates new
   catalog entries.
4. A working AI-powered workflow ("AI Service Health Check") that reads
   catalog data and writes back an AI-generated recommendation.

## Learn more

- [Port documentation](https://docs.getport.io)
- [Port skills](https://docs.port.io/ai-interfaces/skills/)
- [AI agents in Port](https://docs.getport.io/ai-interfaces/ai-agents/overview)
