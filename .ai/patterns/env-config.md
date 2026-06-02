# Env & Config Patterns

Validate environment variables once, at boot, and import a typed object
everywhere. A missing or malformed var should crash startup with a clear
message — never surface as a confusing runtime error three layers deep.

See also: `security.md` (secrets handling), CLAUDE.md "Never commit `.env*`".

## Validate at the boundary

```ts
// src/env.ts — the ONLY place process.env is read
import { z } from "zod"

const schema = z.object({
  NODE_ENV: z.enum(["development", "test", "production"]).default("development"),
  DATABASE_URL: z.string().url(),
  STRIPE_SECRET_KEY: z.string().startsWith("sk_"),
  NEXT_PUBLIC_APP_URL: z.string().url(),       // NEXT_PUBLIC_* = exposed to client
})

const parsed = schema.safeParse(process.env)
if (!parsed.success) {
  console.error("❌ Invalid env:", parsed.error.flatten().fieldErrors)
  throw new Error("Invalid environment variables")   // fail fast at boot
}

export const env = parsed.data                  // typed, validated, autocomplete
```

## Use the typed object, never raw process.env

```ts
// ✅ Do
import { env } from "@/env"
const db = connect(env.DATABASE_URL)

// ❌ Don't: read raw — no validation, no types, typos pass silently
const db = connect(process.env.DATABASE_URL!)   // could be undefined at runtime
```

## Client vs server vars

- Only vars prefixed for the client (`NEXT_PUBLIC_*`, `VITE_*`) reach the browser.
- **Never** put a secret behind a public prefix — it ships in the bundle.

```ts
// ❌ Don't: secret exposed to the client
NEXT_PUBLIC_STRIPE_SECRET_KEY=sk_live_...   // now in every user's browser
// ✅ Do: secret stays server-only; only the publishable key is public
STRIPE_SECRET_KEY=sk_live_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
```

## .env.example is the contract

- Every var in `src/env.ts` has a row in `.env.example` (with a dummy/placeholder
  value and a one-line comment). This is the onboarding source of truth.
- Commit `.env.example`. Never commit `.env`, `.env.local`, etc.

```bash
# .env.example
DATABASE_URL=postgres://user:pass@localhost:5432/app   # local Postgres
STRIPE_SECRET_KEY=sk_test_xxx                           # Stripe test mode key
```

## Negative examples

```ts
// ❌ Don't: scatter process.env across the codebase — no single source of truth
if (process.env.FEATURE_X === "true") {}        // stringly-typed, unvalidated everywhere

// ❌ Don't: default a required secret to empty — hides the misconfig
const key = process.env.API_KEY || ""           // silently runs with no key

// ❌ Don't: read env in client components directly for server config
```

## Project specifics

- **Validation location:** <!-- TODO: src/env.ts (or t3-env / envalid) -->
- **Per-environment config:** <!-- TODO: how dev/staging/prod differ; where set -->
- **Feature flags:** <!-- TODO: env-driven / LaunchDarkly / DB-driven / none -->
