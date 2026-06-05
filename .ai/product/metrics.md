# Metrics & Instrumentation

What we measure and how we capture it. A feature isn't done until the events that
prove it works are firing. Filled during `../prompts/product-discovery.md`.

See also: `brief.md` (success metrics), `../patterns/observability.md` (technical
logs ≠ product analytics — this file is product/business events).

## North star

<!-- TODO: the one metric that best captures delivered value -->
*Example: Confirmed bookings per week.*

## KPIs

<!-- TODO: 3–6 supporting metrics with a target/direction -->

| Metric | Definition | Target |
|--------|------------|--------|
| *Booking completion* | *bookings ÷ booking-starts* | *> 60%* |
| *Operator retention* | *active in week 4* | *> 40%* |
| *Time-to-first-booking* | *signup → first confirmed* | *< 24h* |

## Funnel

The critical path to value — instrument every step so you can see where users drop.

```
<!-- TODO: name each step; these become the events below -->
Visit → View tour → Start booking → Pay → Confirmed
```

## Event instrumentation plan

Track **named events with typed properties** — consistent names, snake_case,
verb_noun. Define them once here; fire from the code as features ship.

| Event | When | Properties |
|-------|------|------------|
| `tour_viewed` | detail page opens | `tour_id`, `source` |
| `booking_started` | form opened | `tour_id`, `seats` |
| `booking_completed` | payment confirmed | `booking_id`, `amount`, `tour_id` |
| `signup_completed` | account created | `role`, `referrer` |

```ts
// ✅ Do: consistent name + structured props (no PII — see security.md)
track("booking_completed", { booking_id, amount, tour_id })

// ❌ Don't: free-form names + ad-hoc props that can't be charted
track("Booking done!!", { user: { email, name } })   // unqueryable + leaks PII
```

## Rules

- Name events `noun_verb` past tense; never rename a live event (breaks history).
- Properties are ids and enums, **not** PII (no raw email/name).
- One funnel step = one event. Don't infer the funnel from page views alone.

## Project specifics

- **Analytics tool:** <!-- TODO: PostHog / Plausible / GA / Amplitude — and the wrapper -->
- **Where `track()` lives:** <!-- TODO: e.g. src/lib/analytics.ts -->
- **Dashboard:** <!-- TODO: link to the funnel/KPI dashboard -->
