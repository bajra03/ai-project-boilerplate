# Observability Patterns

How we make production debuggable: structured logs, metrics, traces. This is the
**single source of truth for logging** — `error-handling.md` points here. The
test: when something breaks at 2am, can you find why from the logs alone?

See also: `error-handling.md` (where errors are caught), `security.md`
(redaction), `../runbooks/incident.md` (using this during an incident).

## Structured logs, not strings

Log JSON objects with stable fields — greppable, filterable, queryable. Never
`console.log` free-form strings in app code.

```ts
// ✅ Do: structured, with context
logger.info("booking.created", { bookingId, userId, tourId, seats })
logger.error("payment.failed", { bookingId, provider: "stripe", err })

// ❌ Don't: unstructured string — can't filter or alert on it
console.log(`Booking ${bookingId} created for ${userId}`)
```

## Levels — use them consistently

| Level | When | Alert? |
|-------|------|--------|
| `error` | Unexpected failure, needs a human | Yes (page on rate spike) |
| `warn` | Degraded but handled (retry, fallback) | Maybe (dashboard) |
| `info` | Key business events (signup, purchase) | No |
| `debug` | Verbose tracing, dev/troubleshooting only | No (off in prod) |

## Correlate with a request ID

Attach one id to every log in a request so you can trace one user's path.

```ts
// middleware: mint/propagate a request id, bind it to the logger
const requestId = req.headers.get("x-request-id") ?? crypto.randomUUID()
const log = logger.child({ requestId })
// every log in this request now carries requestId → filter to see the full path
```

## Redact — never log secrets or PII

```ts
// ❌ Don't: log the whole object (tokens, passwords, full PII leak into logs)
logger.info("login", { user })                  // includes passwordHash, email, token

// ✅ Do: log identifiers, not contents
logger.info("login", { userId: user.id })       // who, not what
```

Redact: passwords/hashes, tokens, API keys, card numbers, full emails/addresses.
When in doubt, log the **id**, not the value. (See `security.md`.)

## What to log (and what not to)

- **Do:** state transitions (`order.paid`), external call outcomes + latency,
  unexpected errors with stack, auth decisions (allow/deny + why).
- **Don't:** success of every trivial read, inside hot loops, anything you'd be
  embarrassed to show a user whose data it is.

## Metrics & traces

- **Metrics** (counts, latencies, error rate) drive dashboards + alerts. Track
  RED for each endpoint: Rate, Errors, Duration.
- **Traces** show a request across services/DB — invaluable for "why is this slow".
- Set alerts on **symptoms users feel** (error rate, p95 latency), not raw CPU.

## Negative examples

```ts
// ❌ Don't: swallow an error without logging (see error-handling.md)
try { await charge() } catch (e) {}

// ❌ Don't: log then rethrow at every layer — duplicate noise for one failure
catch (e) { logger.error(e); throw e }          // log once, at the boundary

// ❌ Don't: log inside a tight loop — floods storage, hides signal
for (const row of millionRows) logger.info("processing", { row })
```

## Project specifics

- **Logger:** <!-- TODO: pino / Winston / console + platform — and where logs ship -->
- **Error tracking:** <!-- TODO: Sentry / Datadog — DSN via env (see env-config.md) -->
- **Metrics / APM:** <!-- TODO: tool + dashboard URL -->
- **Alert routing:** <!-- TODO: where alerts go (PagerDuty / Slack) — see runbooks/incident.md -->
