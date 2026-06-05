# Template Version

Boilerplate version: **1.3.0**
Last updated: 2026-06-02
Source: `~/ai-boilerplate/`

## Changelog

### 1.3.0 — 2026-06-02
- **System patterns:** added `patterns/testing.md`, `security.md`,
  `state-management.md`, `env-config.md`, `observability.md`; added
  `runbooks/incident.md`. Closed TODOs in `error-handling.md` (logging),
  `api-routes.md` (webhooks + rate limiting), `components.md` (forms + data
  fetching), `db-queries.md` (migrations).
- **UI/UX:** expanded `design/design.md` with Layout Patterns, Component State
  Catalog, Motion & Animation, Content & Microcopy, Performance Budget. Added
  `design/ux-flows.md` (IA + core user flows).
- **Product:** added `.ai/product/` — `brief.md`, `metrics.md`, `risks.md`,
  `README.md`. Roadmap template gained per-phase Validation gates, a Pre-phase
  risk spike, and Phase 4 (post-launch: measure & iterate).
- **Wiring:** added `prompts/product-discovery.md`; updated `bootstrap.md`
  (product questions + fill steps + step 4b), `reusable.md` (product brief,
  add-a-test, security review, incident response, refresh metrics, risk review),
  and `roadmap.md` prompt (loads product/ + ux-flows, emits gates + Phase 4).
- Indexed everything in `.ai/README.md`, root `README.md`, and `CLAUDE.md`.

### 1.2.0 — 2026-05-19
- Added `.ai/prompts/bootstrap.md` — structured one-time setup prompt for fresh projects
- Updated root `README.md` "Option 3: Ask Claude" to point at the bootstrap prompt
- Slimmed `.ai/prompts/reusable.md` "Fill .ai/ for a new project" section to a pointer

### 1.1.0 — 2026-05-19
- Added `.ai/design/` folder: `tokens.md`, `components.md`, `a11y.md`, `README.md`
  (later consolidated into a single `design.md` in 1.3.0)
- Indexed in `.ai/README.md`, root `README.md` file legend, and `CLAUDE.md` "When unsure" + "Always"

### 1.0.0 — 2026-05-19
- Initial boilerplate
- Folders: context, patterns, schemas, runbooks, tasks, prompts
- Root `CLAUDE.md` template

## Upgrading

When the source boilerplate bumps its version:

1. `diff` source `.ai/` against this project's `.ai/`
2. Merge new sections, files, or examples
3. Bump this file's version to match
