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

<!-- TODO: how to handle Stripe / external webhooks (verify signature, idempotency key) -->

## Rate limiting

<!-- TODO: which routes are rate-limited and how -->
