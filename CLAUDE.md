# Project: <!-- TODO: project name -->

<!-- TODO: 1–2 sentence description of what this project does and who uses it -->

## Stack

<!-- TODO: list frameworks + versions. Examples:
- Runtime: Node 20 / Bun 1.x
- Framework: Next.js 15 (App Router)
- DB: PostgreSQL 16 via Drizzle ORM
- Auth: Clerk / Supabase / NextAuth
- Styling: Tailwind v4 + shadcn/ui
- Deployment: Vercel
-->

## Commands

```bash
# Dev
pnpm dev               # <!-- TODO: confirm -->

# Build
pnpm build

# Test
pnpm test
pnpm test:e2e

# DB
pnpm db:push           # <!-- TODO -->
pnpm db:studio
```

## Conventions

- **Package manager:** pnpm (never npm or yarn — lockfile mismatch breaks CI)
- **Imports:** Use `@/` path alias, never `../../../`
- **Files:** kebab-case for files, PascalCase for components
- **Types:** Define in `src/types/` and re-export
- **Env:** Read `.env.example` for required vars

## Always

- <!-- TODO: project-specific must-dos -->
- Run typecheck before claiming done
- Match existing patterns — see `.ai/patterns/`
- For UI work, follow `.ai/design/` (tokens, component rules, a11y)

## Never

- <!-- TODO: project-specific don'ts -->
- Don't introduce new dependencies without asking
- Don't commit `.env*` (except `.env.example`)
- Don't use `any` — use `unknown` + narrow

## When unsure

1. Check `.ai/context/architecture.md` for system overview
2. Check `.ai/patterns/` for code templates
3. Check `.ai/design/` for tokens, component rules, a11y baseline
4. Check `.ai/schemas/` for data shapes
5. Check `.ai/tasks/current.md` for in-progress work
6. Ask the user
