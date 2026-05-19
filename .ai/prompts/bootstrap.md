# Bootstrap Prompt — Use Once Per New Project

Run this **immediately after copying the boilerplate** into a fresh project. It tells Claude to scan the code, ask you the minimum questions needed, and fill every TODO across `CLAUDE.md` and `.ai/`.

## How to use

1. Copy the boilerplate into your project (see root `README.md` — Option 1 or 2).
2. Open the project in Claude Code.
3. Paste the prompt below. Replace bracketed `[...]` placeholders. Keep the rest verbatim.

---

## The prompt

```
You are bootstrapping the .ai/ + CLAUDE.md scaffold for this project.
Your job: fill every TODO marker so future Claude sessions start with full context.

PROJECT
- Name: [project name]
- One-liner: [what it does, who uses it]
- Stage: [greenfield / existing codebase / migrating from X]
- Surface: [web app / mobile / API / CLI / library / mix]

PROCESS (follow in order, don't skip)

1. SCAN
   Read these in this order:
   - package.json (or equivalent) → stack, scripts, deps
   - README.md (project root, not boilerplate) if it exists
   - src/ or app/ tree → architecture, patterns already in use
   - .env.example if it exists → required env vars
   - Any existing config: tsconfig, tailwind.config, drizzle.config, etc.

2. ASK (one batch, max 5 questions)
   Only ask what you cannot infer from the code. Likely gaps:
   - Domain glossary terms specific to the business
   - "Why we chose X" rationale for non-obvious tech picks
   - Deployment target if not visible in config
   - Out-of-scope items the AI should NOT propose (goes in tasks/backlog.md rejections)
   - Design source of truth (Figma URL? Storybook? Brand guide?) — only if this is a UI project

   Batch all questions into one message. Wait for answers before writing.

3. FILL
   Replace every <!-- TODO --> across:
   - CLAUDE.md → project name, description, stack, commands, conventions, always/never lists
   - .ai/context/architecture.md → system overview, data flow, components, critical paths
   - .ai/context/tech-stack.md → frameworks + versions + rationale (read from package.json/lock)
   - .ai/context/glossary.md → domain terms from my answers + code (entity names, enums)
   - .ai/patterns/components.md → form pattern, data fetching strategy (if web/mobile)
   - .ai/patterns/api-routes.md → route conventions (if API surface exists)
   - .ai/patterns/db-queries.md → query patterns (if DB exists)
   - .ai/patterns/error-handling.md → error conventions
   - .ai/schemas/database.md → tables (read schema files if present)
   - .ai/schemas/api.md → endpoints (read route files if present)
   - .ai/schemas/types.md → core types (read src/types/ if present)
   - .ai/runbooks/*.md → adjust commands to match this project
   - .ai/tasks/current.md → "Initial setup — bootstrapping context" + today's date
   - .ai/tasks/backlog.md → any "explicitly rejected" items from my answers

4. DESIGN FOLDER DECISION
   - UI project (React/Vue/Svelte/mobile)?
     → Fill .ai/design/tokens.md, components.md, a11y.md using tailwind.config + globals.css if present.
       If design source of truth is Figma/Storybook, reference the URL at the top of tokens.md.
   - Backend / CLI / library only?
     → Ask me first: "Delete .ai/design/ since no UI surface? (y/n)"
       Only delete after I confirm.

5. CREATE FIRST ADR
   Add .ai/context/decisions/0001-stack.md using the template at
   .ai/context/decisions/0000-template.md. Document why this stack was
   chosen (from my answers + what's already in package.json).

6. CLEAN UP
   - Delete this file (.ai/prompts/bootstrap.md) — it's one-time use.
   - Leave .ai/prompts/reusable.md intact.

REPORT (last message)
End with a structured summary:
- Files filled: [list]
- Files left empty (and why): [list]
- Open questions you'd still like answered later: [list, or "none"]
- Suggested next prompt: [point me at .ai/prompts/reusable.md]

RULES
- Don't invent facts. If unsure, leave the TODO and note it in the final report.
- Don't add new dependencies, frameworks, or config files. You're documenting what exists.
- Keep every file under 200 lines (boilerplate principle).
- Match the existing tone in each file (examples first, negative examples, bullet rules).
- Don't touch source code. Documentation only.
```

---

## After bootstrap

- Skim `CLAUDE.md` and `.ai/context/architecture.md` — these are highest-leverage. Correct anything wrong.
- For ongoing work, use the prompts in `.ai/prompts/reusable.md`.
- When you ship something significant, run the "Update docs" prompt from `reusable.md` so context doesn't drift.
