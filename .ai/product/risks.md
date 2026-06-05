# Risk Register

The assumptions that could sink the project, ranked, with a plan to de-risk each.
The point: prove the scary stuff *early* (in a Pre-phase spike) instead of
discovering it at launch. Filled during `../prompts/product-discovery.md`.

See also: `brief.md` § Key assumptions (raw list), `../roadmap.md` (Pre-phase
spikes pull from the high-priority rows here).

## How to use

- Score each risk: **Likelihood** (L/M/H) × **Impact** (L/M/H).
- High × High = de-risk **before** building on top of it. Define a cheap
  experiment that gives a yes/no answer fast.
- Review this table at every phase boundary: close de-risked rows, add new ones.

## Register

<!-- TODO: replace example rows. Cover product, technical, market, and
dependency risks. -->

| # | Risk / assumption | L | I | Mitigation / validation experiment | Owner | Status |
|---|-------------------|---|---|------------------------------------|-------|--------|
| 1 | *Operators won't switch from WhatsApp* | M | H | *5 user interviews + a clickable prototype before building backend* | *PM* | *open* |
| 2 | *Payment provider can't do split payouts* | M | H | *Spike: prototype one payout in test mode in Pre-phase* | *Eng* | *open* |
| 3 | *Demand is seasonal — slow to validate* | M | M | *Launch in peak season; instrument funnel early (metrics.md)* | *PM* | *open* |

## Risk categories to consider

- **Product:** will users want it / change behavior? (validate with people, not code)
- **Technical:** does the hard integration / novel approach actually work? (spike it)
- **Market/timing:** is now the right time; is the audience reachable?
- **Dependency:** third-party API limits, pricing, lock-in, deprecation.

```
// ❌ Don't: build the whole feature on an unproven assumption, then find out at launch
// ✅ Do: smallest experiment that turns the riskiest assumption into a fact, first
```

## Notes for AI

- When a task rests on an `open` High×High risk, flag it and suggest the spike
  before full implementation.
- When a risk is validated/killed, mark it `closed` with the result — don't delete
  (the history is the lesson).
