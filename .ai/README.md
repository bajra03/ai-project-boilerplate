# `.ai/` — Context for AI assistants

Structured project context. Update as the project evolves.

## Folders

- **`context/`** — Architecture, stack, glossary, decisions
- **`patterns/`** — Code templates (how we write things here)
- **`design/`** — Design tokens, component usage rules, a11y baseline (optional for backend-only projects)
- **`schemas/`** — Data shapes (DB, API, types)
- **`runbooks/`** — Step-by-step playbooks
- **`tasks/`** — Current and backlog work
- **`prompts/`** — Reusable prompt templates

## Top-level files

- **`roadmap.md`** — Staged delivery plan (Pre-phase → Phase 1 → 2 → 3). Fill via `prompts/roadmap.md` after bootstrap; re-run when planning V1/V2.

## Update rules

- Keep files **short** (<200 lines). AI scans, not reads.
- Lead with **examples**, not abstract rules.
- Include **negative examples**: "Don't X because Y."
- Update `tasks/current.md` between sessions.
- Update `roadmap.md` when a phase deliverable ships or status changes.
- Add an ADR (`context/decisions/`) for any architectural choice you'd defend in a code review.

## See also

- Root `CLAUDE.md` — auto-loaded, project rules + commands
- `_template-version.md` — boilerplate version (for upgrading)
