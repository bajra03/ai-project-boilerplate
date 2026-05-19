# AI Context Boilerplate

Reusable `.ai/` + `CLAUDE.md` scaffold to make AI assistants (Claude, etc.) work faster and more accurately on your projects.

## What's inside

- **`CLAUDE.md`** — root file, auto-loaded by Claude Code every session
- **`.ai/`** — structured context for AI: architecture, patterns, design, schemas, runbooks

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
2. **Ask** up to 5 clarifying questions in one batch
3. **Fill** every TODO in `CLAUDE.md` and `.ai/`
4. **Decide** whether to keep or delete `.ai/design/` based on project type
5. **Write** the first ADR documenting the stack choice
6. **Clean up** by deleting itself

Output: a structured handoff summary listing what was filled, what's still empty, and what to ask later.

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
| `.ai/context/decisions/` | ADRs — one per major decision |
| `.ai/patterns/*.md` | Code templates: components, APIs, DB, errors |
| `.ai/design/tokens.md` | Color / spacing / typography / breakpoint tokens |
| `.ai/design/components.md` | Design-system component usage rules |
| `.ai/design/a11y.md` | Accessibility baseline (WCAG, keyboard, ARIA) |
| `.ai/schemas/*.md` | Data shapes: DB tables, API contracts, types |
| `.ai/runbooks/*.md` | Step-by-step playbooks: add feature, debug, deploy |
| `.ai/tasks/current.md` | What's in progress now (continuity between sessions) |
| `.ai/tasks/backlog.md` | What's next |
| `.ai/prompts/bootstrap.md` | One-time setup prompt for fresh projects |
| `.ai/prompts/reusable.md` | Prompts you reuse often |

## Principles

1. **Short files.** AI scans, doesn't read. Aim <200 lines per file.
2. **Examples beat rules.** Show, don't tell.
3. **Negative examples too.** "Don't do X because Y."
4. **Commit `.ai/`.** Version-controlled context.
5. **Update `tasks/current.md`** between sessions for continuity.
