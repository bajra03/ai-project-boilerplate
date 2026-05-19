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

<!-- TODO: how migrations are generated and applied. Drizzle: `pnpm db:generate` + `pnpm db:migrate`. Prisma: `prisma migrate dev`. -->
