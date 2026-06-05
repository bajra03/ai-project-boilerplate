# API Route Patterns

How we write backend endpoints. Match this style.

## File layout

```
src/app/api/
├── <resource>/
│   ├── route.ts          # GET (list), POST (create)
│   └── [id]/
│       └── route.ts      # GET (one), PATCH, DELETE
```

## Standard route handler

```ts
// src/app/api/<resource>/route.ts
import { NextRequest } from "next/server"
import { z } from "zod"
import { requireAuth } from "@/lib/auth"
import { db } from "@/lib/db"
import { apiError, apiOk } from "@/lib/api"

const CreateSchema = z.object({
  // <!-- TODO -->
})

export async function POST(req: NextRequest) {
  const user = await requireAuth(req)
  const body = CreateSchema.safeParse(await req.json())
  if (!body.success) return apiError(400, "invalid_body", body.error.flatten())

  const row = await db.insert(/* ... */).returning()
  return apiOk(row[0], { status: 201 })
}
```

## Rules

- **Validate input** with Zod at the boundary. Never trust `req.json()` directly.
- **Auth first.** Call `requireAuth()` before reading body when possible.
- **Use `apiOk` / `apiError` helpers** — uniform shape.
- **Don't catch errors** to log and rethrow. Let the global handler do it.
- **Idempotent endpoints** (PUT, DELETE) must succeed on repeated calls.

## Error shape

```json
{ "error": { "code": "invalid_body", "message": "...", "details": {} } }
```

## Negative examples

```ts
// ❌ Don't: skip validation
const body = await req.json()
await db.insert(body)  // injection / type errors

// ❌ Don't: return raw error objects
return Response.json({ error: err }, { status: 500 })

// ❌ Don't: 200 with `success: false` — use proper status codes
```

## Webhooks

Webhooks are untrusted POSTs from outside. **Verify the signature before doing
anything**, and make handling idempotent — providers retry, so you'll see the
same event more than once.

```ts
// src/app/api/webhooks/stripe/route.ts
export async function POST(req: NextRequest) {
  const raw = await req.text()                          // raw body, not parsed JSON
  const sig = req.headers.get("stripe-signature")!
  let event
  try {
    event = stripe.webhooks.constructEvent(raw, sig, env.STRIPE_WEBHOOK_SECRET)
  } catch {
    return apiError(400, "invalid_signature")           // reject unverified
  }

  if (await alreadyProcessed(event.id)) return apiOk(null)   // idempotency guard
  await handleEvent(event)
  await markProcessed(event.id)
  return apiOk(null)
}
```

- Read the **raw body** for signature verification — don't `req.json()` first.
- Store processed event ids; skip duplicates.
- Return 2xx fast; offload slow work to a job so the provider doesn't retry.

```ts
// ❌ Don't: trust a webhook without verifying the signature
const event = await req.json()   // anyone can POST this
```

## Rate limiting

Protect auth, anything that sends email/SMS, and expensive endpoints (LLM,
search). Key by user id when authed, else by IP. Return **429** with `Retry-After`.

```ts
const { success } = await ratelimit.limit(user?.id ?? ip)
if (!success) return apiError(429, "rate_limited")
```

See `../patterns/security.md` § Rate limiting for policy.

<!-- TODO: project limiter (Upstash / middleware) + per-route limits -->

