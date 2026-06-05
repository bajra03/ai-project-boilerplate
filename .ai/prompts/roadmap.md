# Roadmap Prompt — Run After Bootstrap, Re-Runnable

Fills `.ai/roadmap.md` with project-specific phases. Run it:

- **First time** — right after `bootstrap.md` + `product-discovery.md` complete, to draft Pre-phase + Phase 1–4.
- **Again later** — when you finish MVP and want to plan V1/V2, refresh dates, or replan after a pivot.

The prompt detects whether the roadmap is empty (full fill) or already filled (refresh / extend).

## How to use

1. Make sure `bootstrap.md` has been run (so `CLAUDE.md` + `.ai/context/*` are populated).
2. Paste the prompt below. Replace bracketed `[...]` placeholders. Keep the rest verbatim.

---

## The prompt

```
You are populating .ai/roadmap.md with project-specific phases.
Use existing .ai/ context — don't re-ask things bootstrap already covered.

MODE DETECTION (do this first)
- Read .ai/roadmap.md.
- If every <!-- TODO --> placeholder is still present → MODE=fill (first run).
- If some phases have real content → MODE=refresh (extend / replan).
- State the detected mode in your first message before asking anything.

PROJECT CONTEXT TO LOAD
- CLAUDE.md → stack, commands, conventions
- .ai/context/architecture.md → components, data flow
- .ai/context/tech-stack.md → frameworks
- .ai/product/brief.md → MVP scope + non-goals (Phase scope comes from here)
- .ai/product/risks.md → High×High risks → Pre-phase spikes
- .ai/product/metrics.md → funnel/KPIs (Phase 4 measurement)
- .ai/schemas/database.md + .ai/schemas/api.md → data surfaces (if present)
- .ai/design/design.md → tokens, component rules, a11y (if UI project)
- .ai/design/ux-flows.md → core flows + states (Phase 1 deliverables come from here)
- .ai/runbooks/deploy.md → deploy target
- .ai/tasks/backlog.md → already-rejected scope (don't add to phases)

ASK (one batch, max 5 questions)
Only what you can't infer. Likely gaps:
- MVP one-liner: "what must exist before we'd let a real user try it?"
- Launch window: target date or week-count from today
- Core user flows: 3–5 screens or endpoints that define the happy path
- Hard constraints: deadline, team size, must-have integrations
- Explicit out-of-MVP: features to defer (will land in backlog.md, not roadmap)

If MODE=refresh, also ask:
- Which phases are done? (so we mark them and add new ones)
- Adding V1/V2 phases, or just refreshing existing dates?

Wait for answers before writing.

FILL .ai/roadmap.md

For MODE=fill:
- Pre-phase deliverables → derive from CLAUDE.md "Commands" + tech-stack.md
  (scaffold, lint/format, env, CI, .ai/ context). ADD a risk-spike deliverable
  for each High×High risk in .ai/product/risks.md. Fill the Pre-phase
  "Validation gate" with the kill-criteria for those spikes.
- Phase 1 (frontend + mock data) deliverables → derive from .ai/design/ux-flows.md
  core flows + .ai/design/design.md. Reference src/mocks/ for fixtures. Fill the
  Phase 1 "Validation gate" — put the prototype in front of real/target users
  before backend cost.
- Phase 2 (backend + real data) deliverables → derive from .ai/schemas/database.md
  + .ai/schemas/api.md + auth model.
- Phase 3 (polish + launch) deliverables → derive from .ai/runbooks/deploy.md
  + standard launch checklist (errors, perf, SEO, analytics).
- Phase 4 (post-launch: measure & iterate) deliverables → derive from
  .ai/product/metrics.md (funnel/KPIs live) + .ai/runbooks/incident.md +
  .ai/product/risks.md review. This phase closes the loop — keep it.
- Dates: backfill from launch window. Spread phases proportionally.
- Status: Pre-phase = "in progress", rest = "not started".
- Keep the "Validation gate" blocks the template already has — fill, don't drop them.

For MODE=refresh:
- Mark completed phases with [x] on all deliverables + Status: done + end date.
- Add new phases below "Post-MVP" section (Phase 4, 5, ... or V1, V2).
- Update Dates on the active phase if the user gave new ones.
- Do NOT rewrite already-completed phase content unless user asks.

CROSS-WRITE (always)
- Append a line to .ai/tasks/current.md "Recently completed":
  "Roadmap [drafted | refreshed] — see .ai/roadmap.md"
- For any out-of-MVP items the user listed: add to .ai/tasks/backlog.md "Later"
  or "Won't do" (ask user which, in same batch).
- DO NOT delete .ai/prompts/roadmap.md — this prompt is re-runnable.

RULES
- Don't invent deliverables. If unsure, leave a `<!-- TODO -->` and note in report.
- Keep file <200 lines (boilerplate principle).
  Exception: `.ai/design/design.md` is intentionally long (tokens +
  component rules + a11y baseline with worked examples). Don't trim
  it to fit the cap.
- Match existing tone — concrete deliverables, strict exit criteria.
- Exit criteria must be testable (not "feature feels good").
- Don't touch source code. Documentation only.

REPORT (last message)
- Mode used: fill | refresh
- Phases drafted / updated: [list with date ranges]
- Out-of-MVP items moved to backlog: [list]
- Open questions: [list, or "none"]
- Suggested next prompt:
  "Run Pre-phase from roadmap" (see .ai/prompts/reusable.md → "Run a phase")
```

---

## After roadmap is filled

You'll now use two prompts repeatedly (both in `.ai/prompts/reusable.md`):

- **"Run a phase from roadmap"** — promotes a phase's deliverables into `tasks/current.md`, then executes them one at a time, updating both files as you go.
- **"Mark phase done"** — flips status, runs exit-criteria checks, suggests promoting the next phase.

Re-run **this** roadmap prompt when:
- MVP shipped → drafting V1
- Major pivot → replanning remaining phases
- Dates slipped → refreshing the schedule
