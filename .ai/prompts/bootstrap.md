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

PROJECT (the user fills these in before pasting. If any field still
contains the bracketed `[...]` example text or is blank, treat it as
NOT PROVIDED and ASK for it — see step 0 below.)
- Name: [project name]
- One-liner: [what it does, who uses it]
- Stage: [greenfield / existing codebase / migrating from X]
- Surface: [web app / mobile / API / CLI / library / mix]
- Repo structure: [standard single-app / pnpm-workspaces / turborepo / nx / yarn-workspaces / other]
- Package manager: [pnpm / npm / yarn / bun]
- Runtime: [Node 20 LTS / Node 22 / Bun 1.x / Deno / other]

PROCESS (follow in order, don't skip)

0. PRE-FLIGHT — confirm PROJECT block
   Look at the PROJECT block above. For each field:
   - If the value still contains the bracketed example
     (e.g. `[project name]`, `[standard single-app / pnpm-workspaces / ...]`),
     OR the value is blank,
     → treat it as NOT PROVIDED.

   MANDATORY (ask now, in one batch, before scanning):
   - Name
   - Stage  (greenfield / existing / migrating)
   - Surface (web / mobile / API / CLI / library / mix)
   - Repo structure (standard single-app / pnpm-workspaces / turborepo /
     nx / yarn-workspaces / other)

   SOFT (defer to step 2a if blank — infer first, ask only if not inferable):
   - One-liner, Package manager, Runtime

   Wait for the user's reply, update the PROJECT block in your working
   memory, then proceed to step 1.

1. SCAN
   Read these in this order:
   - package.json (or equivalent) → stack, scripts, deps, packageManager field
   - README.md (project root, not boilerplate) if it exists
   - src/ or app/ tree → architecture, patterns already in use
   - .env.example if it exists → required env vars
   - Any existing config: tsconfig, tailwind.config, drizzle.config, etc.
   - IF monorepo: also read pnpm-workspace.yaml, turbo.json, nx.json,
     root package.json "workspaces" field. List each app/package
     directory and infer its purpose.

2. ASK (one batch — wait for answers before writing)

   2a. ALWAYS ASK (skip any already collected in pre-flight or inferable from code):
       - Confirm package manager and runtime if still not declared
       - Deployment target (Vercel / Fly / Railway / Cloudflare / self-hosted / etc.)
       - Out-of-scope items the AI should NOT propose
         (goes into tasks/backlog.md rejections)
       - Design source of truth (Figma URL / Storybook / brand guide)
         — only if UI surface
       - Domain glossary terms specific to the business
       - "Why we chose X" rationale for non-obvious tech picks
       - Product basics (for .ai/product/, brief — full brainstorm is step 4b):
         core users + problem solved; what success looks like (one metric);
         riskiest assumption (what must be true for this to work)
       - IF monorepo: which app is the primary work surface?
         (ambiguous-only — skip if obvious)

   2b. IF STAGE = GREENFIELD, also present this tech-stack checklist.
       Tell the user: "Reply with your picks. Leave blank or write 'none'
       for any not applicable. Add 'planned' next to anything you intend
       to use but haven't installed yet."

       - Database:           [Postgres / MySQL / SQLite / MongoDB / none]
       - ORM / query builder:[Drizzle / Prisma / Kysely / raw SQL / none]
       - Auth:               [Clerk / Auth.js / Better-Auth / Supabase Auth / custom / none]
       - Payments:           [Stripe / Paddle / LemonSqueezy / none]
       - Email:              [Resend / Postmark / SendGrid / AWS SES / none]
       - File storage:       [S3 / R2 / Supabase Storage / UploadThing / none]
       - Background jobs:    [Inngest / Trigger.dev / BullMQ / cron / none]
       - Monitoring/errors:  [Sentry / Datadog / Highlight / none]
       - Product analytics:  [PostHog / Plausible / GA / Vercel Analytics / none]
       - AI / LLM provider:  [Anthropic / OpenAI / Vercel AI SDK / none]
       - Testing:            [Vitest / Jest / Playwright / Cypress / none]
       - CI/CD:              [GitHub Actions / Vercel / CircleCI / GitLab CI / none]

   2c. IF STAGE = EXISTING: skip the checklist. Stack is inferred from
       package.json. Only ask the 2a items.

3. FILL
   Replace every <!-- TODO --> across:
   - CLAUDE.md → project name, description, stack, commands, conventions, always/never lists
     · Use the declared package manager in commands and Conventions
       (do NOT keep "pnpm (never npm or yarn)" wording if user picked another PM)
     · IF monorepo: commands use workspace-aware forms
       (e.g. `turbo dev`, `pnpm -F <app> dev`, `nx serve <app>`)
   - .ai/context/architecture.md → system overview, data flow, components, critical paths
     · IF monorepo: add a "Workspace layout" section listing each app/package + purpose
   - .ai/context/tech-stack.md → frameworks + versions + rationale (read from package.json/lock)
     · IF greenfield: also write the checklist picks. Mark anything the user
       flagged as `planned` with "(planned — not yet installed)" so future
       Claude does not assume the dep exists.
   - .ai/context/glossary.md → domain terms from my answers + code (entity names, enums)
   - .ai/patterns/components.md → form pattern, data fetching strategy (if web/mobile)
   - .ai/patterns/api-routes.md → route conventions + webhooks/rate-limit (if API surface)
   - .ai/patterns/db-queries.md → query patterns + migration commands (if DB exists)
   - .ai/patterns/error-handling.md → error conventions
   - .ai/patterns/{testing,security,state-management,env-config,observability}.md
     → fill the "Project specifics" block in each from the stack (test runner,
       auth/roles, server-cache lib, env validation, logger/error tracking)
   - .ai/runbooks/incident.md → alert source, status page, rollback owner, escalation
   - .ai/schemas/database.md → tables (read schema files if present)
   - .ai/schemas/api.md → endpoints (read route files if present)
   - .ai/schemas/types.md → core types (read src/types/ if present)
   - .ai/runbooks/*.md → adjust commands to match this project + PM + workspace shape
   - .ai/tasks/current.md → "Initial setup — bootstrapping context" + today's date
   - .ai/tasks/backlog.md → any "explicitly rejected" items from my answers
   - .ai/product/* → seed brief.md / metrics.md / risks.md from the step 2a
     product answers (rough draft — full pass is step 4b)

4. DESIGN FOLDER DECISION
   - UI project (React/Vue/Svelte/mobile)?
     → Run `.ai/prompts/design-discovery.md` end-to-end — a Senior PM + Frontend
       Designer brainstorm: detect category → propose 3–5 directions → user picks
       → record in design.md `## Design Direction` → fill Tokens + Component Rules
       + Accessibility → fill `.ai/design/ux-flows.md` (its step 6): the IA /
       nav map and 3–5 core flows with empty/loading/error/edge states. Roadmap
       Phase 1 is built from these flows.

       Do NOT fill design.md before running the discovery prompt — even if
       Tailwind/shadcn is already configured. Then it confirms the direction
       matches the code (or surfaces a pivot).

   - Backend / CLI / library only?
     → Ask me first: "Delete .ai/design/ since no UI surface? (y/n)"
       Only delete after I confirm.

4b. PRODUCT DISCOVERY
   - Has a product/business surface (most apps)?
     → Run `.ai/prompts/product-discovery.md` end-to-end — Senior PM brainstorm
       that confirms problem/users/job, then fills `product/brief.md`,
       `metrics.md`, `risks.md`. Reuse step 2a answers; don't re-ask. High×High
       risks become Pre-phase spikes in the roadmap.
   - Pure library / internal tool with no product surface?
     → Ask first: "Delete .ai/product/ since no product surface? (y/n)" — delete only after I confirm.

5. CREATE FIRST ADR
   Add .ai/context/decisions/0001-stack.md using the template at
   .ai/context/decisions/0000-template.md. Document why this stack was
   chosen, citing either the checklist picks (greenfield) or the
   inferred stack (existing) explicitly. Include repo-structure rationale
   if monorepo.

6. CLEAN UP
   - Delete this file (.ai/prompts/bootstrap.md) — it's one-time use.
   - Leave .ai/prompts/reusable.md intact.

REPORT (last message)
End with a structured summary:
- Repo structure: [declared / detected]
- Package manager + runtime: [...]
- Files filled: [list]
- Files left empty (and why): [list]
- Stack items marked "planned — not yet installed": [list, or "none"]
- Monorepo docs filled (workspace layout, per-app commands): [yes/no/N/A]
- Product docs filled (brief / metrics / risks): [yes/no/N/A]
- Open questions you'd still like answered later: [list, or "none"]
- Suggested next prompts: `product-discovery.md` (if step 4b skipped), then
  `roadmap.md` — draft Pre-phase + Phase 1–4, scoped from the brief + risks

RULES
- Don't invent facts. If unsure, leave the TODO and note it in the final report.
- Don't add new dependencies, frameworks, or config files. You're documenting what exists or what the user declared as planned.
- Keep every file under 200 lines (boilerplate principle).
  Exception: `.ai/design/design.md` is intentionally long (tokens +
  component rules + a11y baseline with worked examples). Don't trim
  it to fit the cap.
- Match the existing tone in each file (examples first, negative examples, bullet rules).
- Don't touch source code. Documentation only.
```

---

## After bootstrap

- Skim `CLAUDE.md` and `.ai/context/architecture.md` — these are highest-leverage. Correct anything wrong.
- For ongoing work, use the prompts in `.ai/prompts/reusable.md`.
- When you ship something significant, run the "Update docs" prompt from `reusable.md` so context doesn't drift.
