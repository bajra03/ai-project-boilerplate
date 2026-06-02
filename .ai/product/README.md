# `product/` — Why we're building this

Product context that the code can't tell you: the problem, the users, what
success means, and what we're betting on. AI reads this to make decisions aligned
with intent — not just "is it valid code" but "does this serve the goal".

Filled during the product brainstorm (see `../prompts/product-discovery.md`),
which runs alongside design-discovery in the bootstrap flow.

## Files

- **`brief.md`** — the PRD-in-one-page: problem, users + personas, JTBD, value
  prop, success metrics, scope / non-goals, assumptions.
- **`metrics.md`** — north-star + KPIs, the funnel, and the event-instrumentation
  plan (what to track, with which properties).
- **`risks.md`** — risk register: the assumptions that could sink the project, how
  likely/severe, and how we'll de-risk them.

## How it connects

- `../roadmap.md` phases are scoped from `brief.md` (MVP) + `risks.md` (spikes).
- `../design/ux-flows.md` realizes the value prop as screens.
- `metrics.md` events get instrumented as features ship.

## Update rules

- Keep files **short** (<200 lines). Lead with the answer, not preamble.
- Update `brief.md` when scope or target user shifts; log a non-goal when you
  explicitly reject something (so AI stops re-proposing it).
- Revisit `risks.md` at each phase boundary — close de-risked items, add new ones.
- Backend-only / internal tool with no product surface? This folder can stay thin
  or be deleted — AI will see "no product context" and not invent one.
