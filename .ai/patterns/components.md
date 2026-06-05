# Component Patterns

How we write UI components in this project. Match this style — don't invent new patterns.

## File layout

```
src/components/
├── ui/                # shadcn primitives — don't hand-edit
├── <feature>/         # feature-grouped components
│   ├── tour-card.tsx
│   └── tour-card.test.tsx
└── layouts/
```

## Standard component

```tsx
// src/components/<feature>/<name>.tsx
import { cn } from "@/lib/utils"

interface Props {
  // <!-- TODO: define props -->
  className?: string
}

export function ComponentName({ className, ...props }: Props) {
  return (
    <div className={cn("base-classes", className)}>
      {/* ... */}
    </div>
  )
}
```

## Rules

- **Server Components by default.** Add `"use client"` only when you need state, effects, browser APIs, or event handlers.
- **Props interface above the component.** Always named `Props` for the local component (export as `ComponentNameProps` only if consumed externally).
- **Always accept `className`** and merge with `cn()`.
- **No default exports** for components. Named exports only — better tree-shaking, easier refactor.
- **Co-locate tests** as `<name>.test.tsx`.

## Negative examples

```tsx
// ❌ Don't: default export
export default function TourCard() {}

// ❌ Don't: inline-define types
export function TourCard(props: { tourId: string; price: number }) {}

// ❌ Don't: add "use client" preemptively
"use client"
export function StaticHeading() { return <h1>Hi</h1> }
```

## Forms

react-hook-form + Zod. Define the schema once, infer the type, share it with the
API route (see `api-routes.md`). The field's error comes from the resolver — no
hand-rolled validation state.

```tsx
"use client"
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"

const Schema = z.object({ email: z.string().email(), seats: z.number().min(1) })
type Values = z.infer<typeof Schema>

export function BookingForm() {
  const form = useForm<Values>({ resolver: zodResolver(Schema) })
  const onSubmit = form.handleSubmit(async (values) => { /* call mutation */ })
  return (
    <form onSubmit={onSubmit}>
      <input {...form.register("email")} aria-invalid={!!form.formState.errors.email} />
      {form.formState.errors.email && <p className="text-destructive">{form.formState.errors.email.message}</p>}
      <button disabled={form.formState.isSubmitting}>Book</button>
    </form>
  )
}
```

- One Zod schema is the source of truth for type + validation, client and server.
- Wire errors to fields for a11y (`aria-invalid` + message) — see `../design/design.md` § Forms.

```tsx
// ❌ Don't: useState per field + manual validation — drifts from the API contract
const [email, setEmail] = useState(""); const [err, setErr] = useState("")
```

## Data fetching

**Server Components fetch directly through the query layer** — no internal
`fetch("/api/...")` round-trip. Client Components read via the server cache
(see `state-management.md`).

```tsx
// ✅ Server Component — call the query module directly (no HTTP hop)
import { listTours } from "@/db/queries/tours"
export default async function ToursPage() {
  const tours = await listTours({ operatorId })   // runs on the server
  return <TourList tours={tours} />
}

// ❌ Don't: fetch your own API from a Server Component — pointless network hop
const tours = await fetch("https://.../api/tours").then(r => r.json())
```

- Server Component → query module directly. Client Component → react-query/SWR.
- API routes are for **external** callers and client mutations, not server-to-self.
