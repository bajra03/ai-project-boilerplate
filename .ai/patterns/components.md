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

```tsx
// src/components/<feature>/<name>-form.tsx
"use client"
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"
import { Form, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form"
import { Input } from "@/components/ui/input"
import { Button } from "@/components/ui/button"

const Schema = z.object({
  email: z.string().email(),
  name: z.string().min(1),
})
type FormValues = z.infer<typeof Schema>

interface Props {
  onSubmit: (values: FormValues) => Promise<void>
}

export function ExampleForm({ onSubmit }: Props) {
  const form = useForm<FormValues>({ resolver: zodResolver(Schema) })

  async function handleSubmit(values: FormValues) {
    try {
      await onSubmit(values)
    } catch {
      form.setError("root", { message: "Something went wrong. Try again." })
    }
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(handleSubmit)} className="space-y-4">
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email</FormLabel>
              <Input {...field} type="email" autoComplete="email" />
              <FormMessage />
            </FormItem>
          )}
        />
        {form.formState.errors.root && (
          <p className="text-sm text-destructive">{form.formState.errors.root.message}</p>
        )}
        <Button type="submit" disabled={form.formState.isSubmitting}>
          {form.formState.isSubmitting ? "Saving…" : "Submit"}
        </Button>
      </form>
    </Form>
  )
}
```

Rules:
- Schema defined with Zod; resolver via `@hookform/resolvers/zod`.
- Always use shadcn `Form`, `FormField`, `FormItem`, `FormLabel`, `FormMessage` wrappers — don't roll custom error spans.
- Disable submit button while submitting; show in-progress label.
- Surface server-side errors via `form.setError("root", ...)`.
- Forms are always `"use client"` — extract the data-fetching/server-action wrapper as a Server Component parent.

## Data fetching

```tsx
// ✅ Server Component — call DB directly (preferred for reads)
import { getItemById } from "@/db/queries/items"
import { notFound } from "next/navigation"

export async function ItemDetail({ id }: { id: string }) {
  const item = await getItemById(id)
  if (!item) notFound()
  return <div>{item.title}</div>
}

// ✅ Client Component — call API route for mutations
"use client"
import { useRouter } from "next/navigation"

export function DeleteButton({ id }: { id: string }) {
  const router = useRouter()

  async function handleDelete() {
    await fetch(`/api/items/${id}`, { method: "DELETE" })
    router.refresh() // revalidates Server Component tree above
  }

  return <button onClick={handleDelete}>Delete</button>
}
```

Rules:
- **Server Components read, Client Components mutate.**
- Server Components call query functions directly — never `fetch("/api/...")` from server-side code.
- After a mutation, call `router.refresh()` to revalidate the nearest Server Component boundary.
- Never import server-only modules (`@/db/client`, `@/lib/auth`) inside a `"use client"` file.
