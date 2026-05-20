# Roadmap

Staged delivery plan for this project. Update as phases progress.

> See also: `tasks/current.md` (active work), `tasks/backlog.md` (triaged future work).
> Roadmap = **what** ships in each phase. Tasks = **next concrete actions**.

## How to use

- One phase active at a time. Promote items into `tasks/current.md` as you start them.
- Don't move to the next phase until **all exit criteria** are met.
- Update `Status` and `Dates` between sessions so AI has continuity.
- Status values: `not started` | `in progress` | `done` | `paused`

---

## Pre-phase — Project scaffolding

**Goal:** Repo, tooling, and `.ai/` context populated so feature work can start cleanly.

**Deliverables**

- [ ] <!-- TODO: repo init + package manager + lint/format -->
- [ ] <!-- TODO: framework scaffold (Next.js / Vite / etc.) -->
- [ ] <!-- TODO: env vars documented in `.env.example` -->
- [ ] <!-- TODO: `.ai/context/`, `.ai/patterns/`, `.ai/schemas/` filled in -->
- [ ] <!-- TODO: local checks green on empty project (typecheck + lint + test runnable via pnpm) -->

**Exit criteria**

- `pnpm dev` runs locally with no errors
- `pnpm build` + `pnpm test` pass locally
- A new contributor can clone, install, and run in <10 min

**Status:** not started
**Dates:** <!-- TODO: start → end -->

---

## Phase 1 — Frontend with mock data

**Goal:** Full UI flows clickable end-to-end using mock data (no real backend yet). Validates UX before backend cost.

**Deliverables**

- [ ] <!-- TODO: route map / page list -->
- [ ] <!-- TODO: core components per `.ai/design/design.md` (Component Rules section) -->
- [ ] <!-- TODO: mock data fixtures (`src/mocks/`) -->
- [ ] <!-- TODO: empty / loading / error states for every screen -->
- [ ] <!-- TODO: responsive + a11y baseline (per `.ai/design/design.md` Accessibility section) -->
- [ ] <!-- TODO: push repo to GitHub (default branch protected) -->
- [ ] <!-- TODO: remote CI wired (GitHub Actions: typecheck + lint + test on PRs and main) -->
- [ ] <!-- TODO: preview deploy to Vercel (or equivalent host) tracking main branch -->

**Exit criteria**

- Every screen reachable from the nav
- Designer / PM can click through happy path without explanation
- No real API calls — swapping mocks for real fetches is the only Phase 2 frontend change
- Remote CI is green on the default branch
- Preview URL is reachable and shows mock-data flows end-to-end

**Status:** not started
**Dates:** <!-- TODO -->

---

## Phase 2 — Backend + real data integration

**Goal:** Replace mocks with real persistence, auth, and API. Frontend contracts unchanged.

**Deliverables**

- [ ] <!-- TODO: database schema + migrations (`.ai/schemas/database.md`) -->
- [ ] <!-- TODO: API routes wired to DB (`.ai/patterns/api-routes.md`) -->
- [ ] <!-- TODO: auth flow (login / session / protected routes) -->
- [ ] <!-- TODO: swap mock fetches → real endpoints -->
- [ ] <!-- TODO: integration tests on critical paths -->

**Exit criteria**

- Happy path works against real DB in a deployed preview
- Auth-gated routes reject unauthenticated requests
- No `src/mocks/` imports remain in production code paths

**Status:** not started
**Dates:** <!-- TODO -->

---

## Phase 3 — Polish + launch

**Goal:** Ship-ready: observability, performance, edge cases, deploy pipeline.

**Deliverables**

- [ ] <!-- TODO: error tracking + logging configured -->
- [ ] <!-- TODO: performance budget met (LCP / bundle size) -->
- [ ] <!-- TODO: edge cases + error states polished -->
- [ ] <!-- TODO: production deploy + rollback runbook (`.ai/runbooks/deploy.md`) -->
- [ ] <!-- TODO: launch checklist (analytics, SEO, legal) -->

**Exit criteria**

- All P0/P1 bugs closed
- Successful production deploy with monitoring green
- Rollback procedure tested once

**Status:** not started
**Dates:** <!-- TODO -->

---

## Post-MVP

Future phases (V1 features, scaling, new surfaces) live in `tasks/backlog.md` until promoted into a numbered phase here.
