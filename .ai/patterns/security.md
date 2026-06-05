# Security Patterns

Baseline rules so AI-written code is safe by default. Most breaches here come
from skipping **authorization** (not authentication) and trusting input. Match
this style.

See also: `api-routes.md` (validation at the boundary), `error-handling.md`
(don't leak internals), `env-config.md` (secrets), `observability.md` (redaction).

## Authn vs Authz — the #1 gap

- **Authentication** = who are you. **Authorization** = are you allowed to touch
  *this* row. Passing auth is not permission.

```ts
// ❌ Don't: authenticate but skip ownership check — IDOR vulnerability
const user = await requireAuth(req)
const booking = await getBookingById(params.id)   // ANY user can read ANY booking
return apiOk(booking)

// ✅ Do: verify the resource belongs to the caller
const user = await requireAuth(req)
const booking = await getBookingById(params.id)
if (!booking || booking.userId !== user.id) throw new NotFoundError("booking", params.id)
//          404 not 403 — don't reveal that the row exists to a non-owner
```

- Check ownership/role on **every** read and write of a user-scoped resource.
- Prefer scoping in the query (`where userId = ?`) over fetch-then-check.

## Input is hostile

- Validate shape with Zod at the boundary (see `api-routes.md`), then validate
  **semantics**: ranges, ownership, state transitions.
- Never build SQL/HTML/shell strings by concatenation.

```ts
// ❌ Don't: string-built SQL — injection
db.execute(`SELECT * FROM tours WHERE name = '${q}'`)
// ✅ Do: parameterized via the ORM / query builder
db.select().from(tours).where(eq(tours.name, q))

// ❌ Don't: render unsanitized HTML
<div dangerouslySetInnerHTML={{ __html: userBio }} />
// ✅ Do: render as text, or sanitize with a vetted library if HTML is required
<div>{userBio}</div>
```

## OWASP quick baseline

| Risk | Default defense |
|------|-----------------|
| Injection (SQL/NoSQL/command) | Parameterized queries only; never interpolate input |
| XSS | Escape by default; avoid `dangerouslySetInnerHTML`; sanitize if unavoidable |
| CSRF | Same-site cookies + framework CSRF token for cookie-auth mutations |
| SSRF | No user-supplied URLs to server-side fetch; allowlist hosts if required |
| Broken access control | Ownership/role check on every resource (see Authz above) |
| Sensitive data exposure | Redact in logs; never return password hashes / tokens in API bodies |

## Secrets

- Read secrets only via the validated env module (`env-config.md`) — never
  `process.env.X` scattered through code, never hard-coded.
- Never log a secret, token, or full PII record (see `observability.md`).

```ts
// ❌ Don't: hard-code or commit
const stripe = new Stripe("sk_live_abc123")
// ✅ Do: from validated env
const stripe = new Stripe(env.STRIPE_SECRET_KEY)
```

## Rate limiting & abuse

- Rate-limit auth endpoints, anything that sends email/SMS, and expensive
  endpoints (LLM, search). Policy lives in `api-routes.md` § Rate limiting.
- Lock out / back off on repeated auth failures.

## Dependency hygiene

- Don't add a dependency to do something the platform already does (see
  CLAUDE.md "Never"). Each dep is attack surface.
- Pin versions; run the lockfile audit in CI; review transitive additions.

## Negative examples

```ts
// ❌ Don't: trust client-sent role/price/userId
await createOrder({ userId: body.userId, total: body.total })   // attacker sets both
// ✅ Do: derive from the session + server-side price lookup
await createOrder({ userId: user.id, total: await priceFor(body.items) })

// ❌ Don't: leak whether an email exists ("no such user" vs "wrong password")
// ✅ Do: identical response + timing for both cases
```

## Project specifics

- **Auth provider & session model:** <!-- TODO: Clerk / Auth.js / custom; cookie vs bearer -->
- **Roles / permission model:** <!-- TODO: how roles are checked, where defined -->
- **Rate limiter:** <!-- TODO: Upstash / middleware / none — and protected routes -->
- **Secrets store:** <!-- TODO: Vercel env / Doppler / SSM -->
