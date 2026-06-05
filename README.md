# AI Context Boilerplate

Reusable `.ai/` + `CLAUDE.md` scaffold to make AI assistants (Claude, etc.) work faster and more accurately on your projects.

## What's inside

- **`CLAUDE.md`** — root file, auto-loaded by Claude Code every session
- **`.ai/`** — structured context for AI: product, architecture, patterns, design, schemas, runbooks — plus bootstrap + discovery prompts

## How to use

### Option 1: Manual copy

```bash
cp -r ~/ai-boilerplate/.ai /path/to/new-project/
cp ~/ai-boilerplate/CLAUDE.md /path/to/new-project/
```

### Option 2: Degit (cleaner)

If pushed to a git remote:

```bash
npx degit your-user/ai-boilerplate /path/to/new-project
```

### Option 3: Ask Claude

After copying, open the project in Claude Code and paste the bootstrap prompt from `.ai/prompts/bootstrap.md`. It walks Claude through:

1. **Scan** the codebase (package.json, src/, configs)
2. **Ask** clarifying questions in one batch (incl. users / problem / success metric)
3. **Fill** every TODO in `CLAUDE.md` and `.ai/`
4. **Design discovery** — run `.ai/prompts/design-discovery.md` (visual direction + `design/ux-flows.md`), or delete `.ai/design/` if no UI
5. **Product discovery** — run `.ai/prompts/product-discovery.md` (brief / metrics / risks), or delete `.ai/product/` if no product surface
6. **Write** the first ADR documenting the stack choice
7. **Clean up** by deleting itself

Output: a structured handoff summary listing what was filled, what's still empty, and what to ask later. Suggested next: `product-discovery.md` (if skipped) → `roadmap.md`.

## Update workflow

When the boilerplate evolves:

1. Bump `.ai/_template-version.md` in this repo
2. In existing projects, compare versions and merge new sections manually

## File legend

| Path | Purpose |
|------|---------|
| `CLAUDE.md` | Project-wide rules + commands (auto-loaded) |
| `.ai/README.md` | Index of `.ai/` content |
| `.ai/context/architecture.md` | System overview, data flow |
| `.ai/context/tech-stack.md` | Frameworks, versions, why chosen |
| `.ai/context/glossary.md` | Domain terms |
| `.ai/context/decisions/` | ADRs — one per major decision (format in its `README.md`) |
| `.ai/product/README.md` | Index of `product/` — what each file is for, when to update |
| `.ai/product/brief.md` | Problem, users, JTBD, success metrics, scope |
| `.ai/product/metrics.md` | North star, KPIs, funnel, event instrumentation |
| `.ai/product/risks.md` | Risk register — assumptions to de-risk early |
| `.ai/patterns/*.md` | Code templates: components, APIs, DB, errors, testing, security, state, env, observability |
| `.ai/design/design.md` | Tokens, component rules, layout, states, motion, microcopy, perf, a11y |
| `.ai/design/ux-flows.md` | Information architecture + core user flows |
| `.ai/schemas/*.md` | Data shapes: DB tables, API contracts, types |
| `.ai/runbooks/*.md` | Step-by-step playbooks: add feature, debug, deploy, incident |
| `.ai/roadmap.md` | Staged delivery plan (Pre-phase → Phase 1–4) |
| `.ai/tasks/current.md` | What's in progress now (continuity between sessions) |
| `.ai/tasks/backlog.md` | What's next |
| `.ai/prompts/bootstrap.md` | One-time setup prompt for fresh projects |
| `.ai/prompts/product-discovery.md` | PM brainstorm → fills `product/` |
| `.ai/prompts/design-discovery.md` | Designer brainstorm → fills `design/` |
| `.ai/prompts/reusable.md` | Prompts you reuse often |

## Principles

1. **Short files.** AI scans, doesn't read. Aim <200 lines per file.
   Exception: `.ai/design/design.md` — design rules carry worked
   examples that lose value when trimmed.
2. **Examples beat rules.** Show, don't tell.
3. **Negative examples too.** "Don't do X because Y."
4. **Commit `.ai/`.** Version-controlled context.
5. **Update `tasks/current.md`** between sessions for continuity.
