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

```ts
// src/app/api/webhooks/<provider>/route.ts
import { headers } from "next/headers"
import { NextRequest } from "next/server"

export async function POST(req: NextRequest) {
  const body = await req.text() // must read as raw text before parsing
  const sig = headers().get("stripe-signature") ?? ""

  let event: Stripe.Event
  try {
    event = stripe.webhooks.constructEvent(body, sig, process.env.STRIPE_WEBHOOK_SECRET!)
  } catch {
    return apiError(400, "invalid_signature")
  }

  // Idempotency: skip if already processed
  const already = await db.query.webhookEvents.findFirst({ where: eq(webhookEvents.externalId, event.id) })
  if (already) return apiOk({ received: true })

  await db.insert(webhookEvents).values({ externalId: event.id, type: event.type })

  switch (event.type) {
    case "payment_intent.succeeded":
      // handle...
      break
    default:
      // ignore unknown events — don't throw
  }

  return apiOk({ received: true })
}
```

Rules:
- **Verify signature** before touching the body — reject unknown callers.
- **Idempotency key** = provider's event ID. Store it; skip duplicate deliveries.
- **Return 200 fast.** Vendors retry on non-2xx. Do heavy work in a background job.
- Raw body must be read as `text()` — do not call `req.json()` first.

## Rate limiting

```ts
// src/lib/ratelimit.ts — using Upstash Ratelimit (swap for any provider)
import { Ratelimit } from "@upstash/ratelimit"
import { Redis } from "@upstash/redis"

export const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, "10 s"),
})

// In a route handler:
const ip = req.ip ?? req.headers.get("x-forwarded-for") ?? "anon"
const { success, reset } = await ratelimit.limit(ip)
if (!success) {
  return apiError(429, "rate_limited", { retryAfter: reset })
}
```

Which routes to rate-limit:
- All auth endpoints (`/api/auth/*`)
- Public-facing POST/PUT endpoints (forms, contact, magic-link)
- Any endpoint that sends email / SMS

Which routes to skip: internal callbacks, health checks, webhooks (rate-limited by provider).
