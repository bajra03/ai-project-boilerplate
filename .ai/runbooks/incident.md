# Runbook: Incident Response

When production is broken or degraded. Goal: **stop user pain first, find root
cause second.** Mitigate before you diagnose. Update with project-specific details.

See also: `deploy.md` (rollback mechanics), `../patterns/observability.md`
(logs/metrics/alerts to look at), `../patterns/error-handling.md`.

## 1. Detect & declare

Triggered by an alert, a user report, or a dashboard spike. Declare an incident
explicitly so everyone knows it's active — don't debug silently.

## 2. Triage — set severity

| Sev | Meaning | Example | Response |
|-----|---------|---------|----------|
| SEV1 | Hard down / data loss / security breach | Site 500s, payments failing, data leak | All hands, mitigate now |
| SEV2 | Major feature broken, no workaround | Checkout broken, login failing | Urgent, drop other work |
| SEV3 | Degraded / minor / has workaround | Slow page, one report, cosmetic | Normal priority |

When unsure, **round up** — easier to downgrade than to under-react.

## 3. Mitigate — stop the bleeding

Restore service before understanding the cause. In rough order of preference:

1. **Roll back** the suspect deploy (fastest, safest) — see `deploy.md` § Rollback.
2. **Disable** the offending feature via flag, if gated.
3. **Scale / restart** if it's resource exhaustion.
4. **Apply a hotfix** only if rollback isn't possible.

```
// ✅ Do: roll back first, investigate the bad build offline
// ❌ Don't: debug root cause live while users keep hitting the error
```

## 4. Communicate

- Post status + severity where the team watches (<!-- TODO: channel -->), update on
  a cadence (e.g. every 30 min for SEV1) until resolved.
- If users are affected, update the status page / notify support.
- One person owns comms so responders can focus on the fix.

## 5. Verify recovery

- Confirm the symptom is gone in metrics, not just "looks fine" — error rate and
  p95 back to baseline (see `../patterns/observability.md`).
- Smoke-test the affected user flow yourself.

## 6. Postmortem (within ~48h, blameless)

Write it down so it can't recur. Keep it short:

- **Timeline** — detection → mitigation → resolution (with timestamps).
- **Impact** — who/what/how long.
- **Root cause** — the real one, not "human error". Ask "why" until it's systemic.
- **What went well / poorly** — detection speed, tooling gaps.
- **Action items** — each with an owner and a due date; track in `../tasks/backlog.md`.

```
// ❌ Don't: blame a person ("X pushed a bad commit")
// ✅ Do: fix the system that let it ship (no test, no canary, no alert)
```

## Project specifics

- **Alert source & routing:** <!-- TODO: PagerDuty / Sentry / Slack — see observability.md -->
- **Status page:** <!-- TODO: URL, or "none" -->
- **Rollback command/owner:** <!-- TODO: who can deploy/rollback prod -->
- **Escalation path:** <!-- TODO: who to wake for SEV1 -->
- **Postmortem template/location:** <!-- TODO: where postmortems are stored -->
