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
- For UI work, follow `.ai/design/design.md` (tokens, component rules, a11y)
- Write tests per `.ai/patterns/testing.md`; ship a regression test with each bug fix
- Check authz (ownership/role), not just authn — see `.ai/patterns/security.md`

## Never

- <!-- TODO: project-specific don'ts -->
- Don't introduce new dependencies without asking
- Don't commit `.env*` (except `.env.example`)
- Don't use `any` — use `unknown` + narrow

## When unsure

1. Check `.ai/context/architecture.md` for system overview
2. Check `.ai/product/brief.md` for the problem, users, and what success means
3. Check `.ai/patterns/` for code templates (components, API, DB, testing, security, state, env, observability)
4. Check `.ai/design/design.md` + `.ai/design/ux-flows.md` for visual rules and user flows
5. Check `.ai/schemas/` for data shapes
6. Check `.ai/roadmap.md` for current phase + planned deliverables
7. Check `.ai/tasks/current.md` for in-progress work
8. Ask the user
