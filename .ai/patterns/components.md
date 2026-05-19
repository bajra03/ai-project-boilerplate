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

<!-- TODO: form pattern (react-hook-form + Zod), one canonical example -->

## Data fetching

<!-- TODO: how Server Components fetch (direct DB call vs API route) -->
