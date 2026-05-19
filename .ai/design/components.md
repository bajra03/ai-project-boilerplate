# Design-System Component Rules

How to **use** components. For how to write them, see `../patterns/components.md`.

Component library: <!-- TODO: e.g. shadcn/ui in `src/components/ui/` -->

## shadcn discipline

shadcn primitives are **owned code**, but treat them like a sealed library — extend via variants, don't restyle inline.

```tsx
// ✅ Add a new variant in the component file using cva
const buttonVariants = cva("...", {
  variants: {
    variant: { default: "...", destructive: "...", ghost: "...", brand: "..." },
  },
})

// ✅ Consume the variant
<Button variant="brand">Book now</Button>

// ❌ Don't: restyle a primitive inline
<Button className="bg-purple-500 hover:bg-purple-600 text-white">Book now</Button>

// ❌ Don't: edit ui/button.tsx for a one-off — add a variant instead
```

## Composition over configuration

Build new components by composing existing ones. Only create a new primitive when an existing one truly can't represent the case.

```tsx
// ✅ Compose
export function TourCard({ tour }: Props) {
  return (
    <Card>
      <CardHeader><CardTitle>{tour.name}</CardTitle></CardHeader>
      <CardContent>{tour.summary}</CardContent>
      <CardFooter><Button>Book</Button></CardFooter>
    </Card>
  )
}

// ❌ Don't: rebuild Card markup from scratch
export function TourCard({ tour }: Props) {
  return (
    <div className="rounded-lg border bg-card p-6 shadow-sm">
      <h3 className="text-lg font-semibold">{tour.name}</h3>
      {/* ... */}
    </div>
  )
}
```

## Naming

`<Feature><Element>` — feature first so related components sort together.

```
TourCard, TourCardSkeleton, TourList, TourFilters
BookingForm, BookingFormSubmit, BookingSummary
```

```tsx
// ❌ Don't: generic names that collide across features
Card, Form, Summary, List
```

## Slots & polymorphism

Use Radix `asChild` to change the rendered tag without losing component behavior.

```tsx
// ✅ Link styled as a Button
<Button asChild>
  <Link href="/tours">Browse tours</Link>
</Button>

// ❌ Don't: duplicate button styles on an anchor
<Link href="/tours" className="inline-flex items-center rounded-md bg-primary px-4 py-2 text-primary-foreground">
  Browse tours
</Link>
```

Fix the tag (no `as` / `asChild` prop) when semantics matter and shouldn't be overridable (e.g. `<form>`, `<dialog>`).

## Conditional classes

Up to ~3 conditional branches → inline `cn()` is fine. More than that → extract a `cva` variant.

```tsx
// ✅ Few branches
<div className={cn("rounded-md p-4", isActive && "bg-accent", isError && "border-destructive")}>

// ❌ Too many branches — extract a variant
<div className={cn(
  "rounded-md p-4",
  size === "sm" && "text-sm p-2",
  size === "md" && "text-base p-4",
  size === "lg" && "text-lg p-6",
  variant === "ghost" && "bg-transparent",
  variant === "solid" && "bg-primary text-primary-foreground",
  disabled && "opacity-50 pointer-events-none",
)}>
```

## Anti-patterns

- **Inline color/spacing arbitrary values** — `bg-[#abc]`, `p-[13px]`. Use tokens.
- **Deeply nested `cn()`** — if you're calling `cn(cn(...))`, refactor.
- **Wrapper soup** — `<div><div><div>` to get spacing. Use `gap-*` on a parent flex/grid instead.
- **One-off shadcn forks** — copy-pasting `Button` into `MyButton` to tweak styles. Add a variant.

## Project-specific rules

<!-- TODO: project-specific patterns — e.g. "all icons from lucide-react, size 16/20/24 only", "use <Image> from next/image, never <img>", etc. -->
