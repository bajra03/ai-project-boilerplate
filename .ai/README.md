# `.ai/` — Context for AI assistants

Structured project context. Update as the project evolves.

## Folders

- **`context/`** — Architecture, stack, glossary, decisions
- **`product/`** — Why we're building this: brief, metrics, risks (optional for pure-library projects)
- **`patterns/`** — Code templates: components, API, DB, errors, testing, security, state, env, observability
- **`design/`** — Design tokens, component rules, a11y baseline, UX flows (optional for backend-only projects)
- **`schemas/`** — Data shapes (DB, API, types)
- **`runbooks/`** — Step-by-step playbooks (add-feature, debug, deploy, incident)
- **`tasks/`** — Current and backlog work
- **`prompts/`** — Reusable prompt templates

## Top-level files

- **`roadmap.md`** — Staged delivery plan (Pre-phase → Phase 1 → 2 → 3). Fill via `prompts/roadmap.md` after bootstrap; re-run when planning V1/V2.

## Update rules

- Keep files **short** (<200 lines). AI scans, not reads.
  Exception: `design/design.md` — design rules need worked examples
  that don't compress without losing value.
- Lead with **examples**, not abstract rules.
- Include **negative examples**: "Don't X because Y."
- Update `tasks/current.md` between sessions.
- Update `roadmap.md` when a phase deliverable ships or status changes.
- Add an ADR (`context/decisions/`) for any architectural choice you'd defend in a code review.

## See also

- Root `CLAUDE.md` — auto-loaded, project rules + commands
- `_template-version.md` — boilerplate version (for upgrading)
