# Core Types

Shared TypeScript types used across the app. The single source of truth.

## Where types live

- **DB row types:** inferred from schema, exported from `src/db/types.ts`
- **API request/response types:** `src/types/api.ts`
- **Domain types** (not 1:1 with DB): `src/types/domain.ts`
- **Zod schemas:** co-located with route handlers; type derived via `z.infer`

## Convention

```ts
// src/db/types.ts
import { tours } from "@/db/schema"
import type { InferSelectModel, InferInsertModel } from "drizzle-orm"

export type Tour = InferSelectModel<typeof tours>
export type NewTour = InferInsertModel<typeof tours>
```

## Naming

- **Entity:** `Tour`, `Booking` — what comes out of the DB
- **`New<Entity>`:** insert shape
- **`<Entity>Update`:** partial patch shape
- **`<Entity>WithRelations`:** joined / hydrated shape
- **`<Verb><Resource>Input`:** API/action input — `CreateBookingInput`

## Negative examples

```ts
// ❌ Don't: redeclare DB types manually
interface Tour { id: string; title: string }  // drifts from schema

// ❌ Don't: use `any` for "shape we'll figure out later"
const data: any = await fetchSomething()

// ❌ Don't: export Zod schemas as types directly
export type CreateInput = typeof CreateSchema  // wrong — use z.infer
```

## Core domain types

<!-- TODO: fill in the types central to this project. They should match names in src/db/schema.ts and src/types/.

Example format:

| Type | Source | Description |
|------|--------|-------------|
| `User` | `src/db/types.ts` | Authenticated account. Row from `users` table. |
| `Session` | `src/db/types.ts` | Active auth session. Row from `sessions` table. |
| `<Entity>` | `src/db/types.ts` | … |
| `<Entity>WithRelations` | `src/types/domain.ts` | Hydrated shape with joined data. |
| `Create<Entity>Input` | `src/types/api.ts` | Validated POST body shape (via Zod). |

After bootstrap, replace this block with the actual types table.
-->
