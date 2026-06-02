# Reusable Prompts

Prompts you find yourself reaching for repeatedly. Copy-paste, fill blanks.

## Fill `.ai/` for a new project

One-time setup after copying the boilerplate. Use the structured prompt in `./bootstrap.md` — it walks Claude through scan → ask → fill → first ADR → cleanup.

## Fill or refresh the product brief

Run **after** bootstrap (or when the product pivots / target user shifts). Use the prompt in `./product-discovery.md` — a Senior PM brainstorm that confirms problem/users/job, then fills `.ai/product/brief.md`, `metrics.md`, `risks.md`. High×High risks become Pre-phase spikes in the roadmap.

## Fill or refresh the roadmap

Run **after** bootstrap and product discovery, and again later when planning V1/V2 or replanning after a pivot. Use the prompt in `./roadmap.md` — it scans existing `.ai/` context (incl. `product/brief.md` + `design/ux-flows.md`), asks max 5 questions, fills `.ai/roadmap.md` (or extends it), and updates `tasks/current.md` + `tasks/backlog.md`.

## Run a phase from roadmap

```
Run <Pre-phase | Phase 1 | Phase 2 | Phase 3> from .ai/roadmap.md.

1. Read the phase's Goal, Deliverables, Exit criteria.
2. Promote unchecked deliverables into .ai/tasks/current.md "In progress".
3. Set the phase Status to "in progress" in roadmap.md (add start date if missing).
4. Work one deliverable at a time. After each:
   - Check it off in roadmap.md ([ ] → [x])
   - Move it to "Recently completed" in tasks/current.md
   - Show me the diff, wait for "next" before continuing
5. When all deliverables checked, run exit criteria as a checklist and report which pass / fail.

Don't skip a deliverable to chase the next one. Don't mark the phase done — I'll do that after exit criteria pass.
```

## Mark phase done

```
Mark <phase> done in .ai/roadmap.md:
- Status: done, set end date
- Verify all deliverables [x] and exit criteria met (list any gap)
- Append "Phase X complete" to tasks/current.md "Recently completed"
- Suggest promoting the next phase (don't promote without confirmation)
```

## Add a feature

```
Add <feature> following .ai/runbooks/add-feature.md.
Match patterns in .ai/patterns/*.
Don't introduce new dependencies without asking.
Show me a step-by-step plan first, before writing code.
```

## Debug

```
Help me debug <symptom>. Reproduce: <steps>.
Follow .ai/runbooks/debug.md — form a hypothesis before changing code.
Don't fix at the symptom; find the root cause.
```

## Refactor

```
Refactor <code> to <goal>. Don't change behavior.
Run typecheck + tests after each step. Stop and show me if any test fails.
```

## Code review

```
Review the diff. Focus on:
1. Does it match patterns in .ai/patterns/?
2. Are there hidden assumptions / unhandled edge cases?
3. Is anything in CLAUDE.md "Never" being violated?

Be specific — file:line for each comment.
```

## Update docs

```
I just shipped <change>. Update .ai/ to reflect it:
- schemas/database.md if tables changed
- schemas/api.md if endpoints changed
- patterns/ if a new pattern emerged
- design/design.md if tokens, component library, or brand source changed
- tasks/current.md to mark done

Don't update files that didn't actually change.
```

## Add a test

```
Write tests for <unit/feature> following .ai/patterns/testing.md.
Mock only the external boundary; run real DB/logic. Name tests by behavior.
Cover the success path + the main failure. Show me the tests before running.
```

## Security review

```
Review <diff/feature> against .ai/patterns/security.md. Check specifically:
1. Authz — ownership/role checked on every user-scoped read + write (not just authn)?
2. Input — validated shape AND semantics; no string-built SQL/HTML?
3. Secrets/PII — nothing logged or returned that shouldn't be?
Flag each issue with file:line + the fix.
```

## Incident response

```
Production is <symptom>. Follow .ai/runbooks/incident.md.
1. Set severity. 2. Mitigate first (rollback/flag) — see runbooks/deploy.md.
3. Then find root cause from logs/metrics (.ai/patterns/observability.md).
Don't debug live while users are hitting the error.
```

## Refresh metrics

```
Update .ai/product/metrics.md to match what's shipped.
- Add events for new funnel steps / key actions (noun_verb, typed props, no PII)
- Reconcile KPIs with what's actually instrumented
Don't rename live events. Show the diff before saving.
```

## Risk review

```
Review .ai/product/risks.md against current state.
- Close de-risked rows (mark validated/killed + result, don't delete)
- Add risks that emerged; score L×I; propose a cheap validation experiment
- Flag any new High×High that should be a roadmap spike before more is built on it.
```

## Refresh design rules

Run when Tailwind config, theme CSS vars, the component library, or the brand source-of-truth has changed since the last `.ai/design/design.md` update.

````
Refresh .ai/design/design.md to match current code.

Re-scan:
- tailwind.config.* and globals.css / theme files → token map, fonts,
  radii, shadows, breakpoints, z-scale
- src/components/ui/ (or wherever the design system lives) → component
  library line + project-specific rules
- Brand / Figma source-of-truth URL (if changed)

Update only sections that drifted. Don't touch the WCAG baseline unless
we've made a deliberate exception (document inline if so).
Show the diff before saving.
````

## Rediscover design direction

Run when the product is pivoting, rebranding, or the current visual identity no longer fits. Restarts the brainstorm and overwrites the Design Direction block.

```
Re-run .ai/prompts/design-discovery.md.
After I pick a new direction, overwrite design.md's `## Design Direction`
section and refresh Tokens + Component Rules to match.
Don't touch the Accessibility baseline.
```

## Custom

<!-- TODO: add prompts you reuse in this project -->
