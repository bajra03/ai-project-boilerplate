# DB Query Patterns

How we read/write the database. Match this style.

## Location

```
src/db/
├── schema.ts          # ORM schema (Drizzle / Prisma)
├── client.ts          # exported `db` client
└── queries/
    ├── tours.ts       # one file per resource
    └── bookings.ts
```

## Standard query module

```ts
// src/db/queries/tours.ts
import { db } from "@/db/client"
import { tours } from "@/db/schema"
import { eq } from "drizzle-orm"
import type { Tour, NewTour } from "@/db/types"

export async function getTourById(id: string): Promise<Tour | null> {
  const rows = await db.select().from(tours).where(eq(tours.id, id)).limit(1)
  return rows[0] ?? null
}

export async function listTours({ operatorId }: { operatorId: string }) {
  return db.select().from(tours).where(eq(tours.operatorId, operatorId))
}

export async function createTour(input: NewTour) {
  const [row] = await db.insert(tours).values(input).returning()
  return row
}
```

## Rules

- **One file per resource** under `queries/`.
- **Named functions, not classes.** Prefix with verb: `get*`, `list*`, `create*`, `update*`, `delete*`.
- **Return typed rows.** Export inferred types from `schema.ts`.
- **No business logic in queries.** Validation, auth, side-effects belong in route handlers or services.
- **Use transactions** when mutating multiple tables that must succeed together.

## Transactions

```ts
await db.transaction(async (tx) => {
  await tx.insert(bookings).values(booking)
  await tx.update(tours).set({ availableSeats: sql`available_seats - 1` }).where(eq(tours.id, tourId))
})
```

## Negative examples

```ts
// ❌ Don't: query the DB from a component without going through queries/
import { db } from "@/db/client"
const tours = await db.select().from(tours)  // inline DB code spreads everywhere

// ❌ Don't: use ORM in API handler when a query function exists
// Add to queries/ first, then call it
```

## Migrations

```bash
# Drizzle
pnpm db:generate    # diff schema.ts → generate SQL migration file
pnpm db:migrate     # apply pending migrations to DB
pnpm db:studio      # open Drizzle Studio (browser DB viewer)

# Prisma
pnpm exec prisma migrate dev     # generate + apply in dev (creates migration file)
pnpm exec prisma migrate deploy  # apply only in prod / CI (no generation)
pnpm exec prisma studio          # open Prisma Studio
```

Rules:
- Migrations are **append-only** — never edit a migration file once committed.
- Always review generated SQL before applying — generators miss nuanced constraints.
- Destructive changes (drop column, rename) require a **multi-step strategy**:
  1. Add new column (deploy)
  2. Backfill data (run script)
  3. Update code to use new column (deploy)
  4. Drop old column (deploy, separate PR)
- Run `db:migrate` in CI against a real DB; never skip in test pipelines.
