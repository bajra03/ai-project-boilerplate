# Product Discovery — Brainstorm Before Filling product/

Run BEFORE filling `.ai/product/brief.md`, `metrics.md`, `risks.md` for the first
time. Re-run when the product pivots or the target user shifts. Runs alongside
`design-discovery.md` in the bootstrap flow (product first, then design).

## Your role

Act as a Senior Product Manager. Don't generate a brief instantly — interrogate
the problem with the user first, then write. A wrong brief sends every later AI
decision the wrong way, so it's worth the questions.

## Process

### 1. SCAN for signal

From PROJECT.One-liner, the README, scanned routes/models, and any answers from
bootstrap, draft a *hypothesis* of: problem, who it's for, and the core job. You'll
confirm — don't assume.

### 2. ASK (one batch — wait for answers)

Keep it to the few questions that actually change the build:

1. **Problem** — "What's broken today for these users, and how do they cope now?"
2. **Users** — "Who are the 1–3 types of people who'll use this? Which one matters most?"
3. **Core job** — "When [situation], they want to [do what], so they can [outcome]?"
4. **Success** — "How will you know in 90 days it's working? One number if you can."
5. **Non-goals** — "What are you deliberately NOT doing in v1?"
6. **Riskiest assumption** — "What has to be true for this to work that you're least sure of?"

If the user already answered some during bootstrap, skip those — don't re-ask.

### 3. FILL `product/brief.md`

Replace every TODO. Be concrete and specific — no generic SaaS filler.

- **Problem** — 2–3 sentences grounded in the user's words
- **Personas** — the table; mark the primary one
- **Jobs to be done** — the JTBD sentence(s)
- **Value proposition** — one sentence, sharper than "a tool for X"
- **Success metrics** — lead metric + supporting (link to metrics.md)
- **Scope** — In (MVP) and Out/non-goals; mirror non-goals into `../tasks/backlog.md` "Won't do"
- **Key assumptions** — the list; the risky ones flow into risks.md

### 4. FILL `product/metrics.md`

- **North star** — the single metric that captures delivered value
- **KPIs** — 3–6 supporting, each with a target/direction
- **Funnel** — name each step from first touch to value
- **Event plan** — one `noun_verb` event per funnel step + key actions, with typed,
  PII-free properties. These get instrumented as features ship.
- Project specifics: analytics tool (from bootstrap stack), where `track()` lives

### 5. FILL `product/risks.md`

- Turn each risky assumption into a register row: Likelihood × Impact, a cheap
  validation experiment, an owner.
- Flag any **High × High** — these become **Pre-phase risk spikes** in `../roadmap.md`.
- Cover product, technical, market/timing, and dependency risks.

## What this is NOT

- Don't write a brief the user didn't confirm — ask, then fill.
- Don't pad with generic personas or vanity metrics (page views ≠ value).
- Don't let a High×High assumption pass without a spike in the roadmap.
- Don't put PII in the event plan (see `../patterns/security.md`).

## Handoff

End by pointing the user at:
- `design-discovery.md` (if UI surface and not yet run)
- `roadmap.md` — now scoped from the brief (MVP) and risks (spikes)
