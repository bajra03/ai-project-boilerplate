# Design Rules for AI

Lock the visual language before the first component is written. Prevents AI drift: random Tailwind classes, off-palette colors, ad-hoc spacing, missing focus states.

Three sections below: **Tokens** (values), **Component Rules** (usage), **Accessibility** (baseline). Backend-only project? Leave the file empty or delete it — AI will see "no rules" and not invent any.

See also: `../patterns/components.md` (React component file/code conventions), `../context/tech-stack.md` (confirms styling stack).

---

# Design Tokens

Single source of truth for visual primitives. **Always reference tokens — never hard-code values.**

Source of truth: <!-- TODO: e.g. `tailwind.config.ts` + `src/app/globals.css` CSS vars -->

## Color

Use **semantic** tokens, not raw color names. Semantic tokens auto-flip in dark mode.

```tsx
// ✅ Use semantic tokens
<div className="bg-background text-foreground border-border">

// ❌ Don't: hard-coded hex / arbitrary values
<div className="bg-[#1a1a1a] text-white">

// ❌ Don't: raw palette names (no dark-mode support)
<div className="bg-slate-900 text-slate-50">
```

Token map: <!-- TODO: list semantic tokens — background, foreground, primary, secondary, muted, accent, destructive, border, input, ring, card, popover -->

## Spacing

Tailwind 4px base scale. Use these steps, no arbitrary values.

| Step | px | Use for |
|------|----|---------|
| `1` | 4 | Icon padding, tight gaps |
| `2` | 8 | Inline gaps, badge padding |
| `3` | 12 | Form field internal padding |
| `4` | 16 | Default card padding, button padding-x |
| `6` | 24 | Section gaps, card padding |
| `8` | 32 | Block spacing |
| `12` | 48 | Page section gaps |
| `16` | 64 | Hero spacing |

```tsx
// ❌ Don't
<div className="p-[18px] mt-[13px]">

// ✅ Do
<div className="p-4 mt-3">
```

## Typography

Families: <!-- TODO: e.g. sans = Inter, mono = JetBrains Mono -->

Scale:

| Token | Use for |
|-------|---------|
| `text-xs` | Captions, helper text |
| `text-sm` | Body small, form labels |
| `text-base` | Body default |
| `text-lg` | Lead paragraph |
| `text-xl` | Card titles |
| `text-2xl` | Section headings |
| `text-3xl` / `text-4xl` | Page titles, hero |

Weights: `font-normal` (400), `font-medium` (500), `font-semibold` (600). **Don't use `font-bold` (700)** unless explicitly part of the brand — `font-semibold` is the default emphasis.

## Radius

| Token | Use for |
|-------|---------|
| `rounded-sm` | Tags, badges |
| `rounded-md` | Inputs, buttons (default) |
| `rounded-lg` | Cards, dialogs |
| `rounded-xl` | Hero panels, feature cards |
| `rounded-full` | Avatars, pills, icon buttons |

## Shadows / elevation

| Token | Use for |
|-------|---------|
| `shadow-sm` | Cards at rest |
| `shadow-md` | Hovered cards, dropdowns |
| `shadow-lg` | Popovers, menus |
| `shadow-xl` | Modals, dialogs |

Don't stack shadows or use arbitrary `shadow-[...]`.

## Breakpoints

Tailwind defaults — **mobile-first**. Start unprefixed, add `md:`/`lg:` to scale up.

| Prefix | Min width | Target |
|--------|-----------|--------|
| (none) | 0 | Mobile |
| `sm:` | 640px | Large phone |
| `md:` | 768px | Tablet |
| `lg:` | 1024px | Laptop |
| `xl:` | 1280px | Desktop |
| `2xl:` | 1536px | Large desktop |

```tsx
// ✅ Mobile-first
<div className="flex flex-col gap-4 md:flex-row md:gap-8">

// ❌ Don't: desktop-first with max-width queries
<div className="flex flex-row gap-8 max-md:flex-col max-md:gap-4">
```

## Dark mode

Strategy: **CSS variables flip in `:root` vs `.dark`** — semantic tokens resolve automatically. Don't sprinkle `dark:` overrides through components.

```tsx
// ✅ Token does the work
<div className="bg-card text-card-foreground">

// ❌ Don't: per-component dark overrides
<div className="bg-white text-black dark:bg-zinc-900 dark:text-zinc-50">
```

Exception: a `dark:` override is fine for an asset that genuinely needs to swap (e.g. logo image).

## Z-index scale

Use named layers — never invent `z-[9999]`.

| Token | Layer |
|-------|-------|
| `z-10` | Sticky elements (table headers) |
| `z-20` | Dropdowns, popovers |
| `z-30` | Sticky nav |
| `z-40` | Modals, dialogs |
| `z-50` | Toasts, top-level overlays |

<!-- TODO: if project uses a custom z scale, replace and document here -->

---

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

---

# Accessibility Baseline

WCAG 2.1 AA minimum. **Never skip these — fixing a11y later is 10× harder.**

## Keyboard

Every interactive element must be reachable by Tab and operable by Enter/Space.

```tsx
// ✅ Native button — keyboard works for free
<button onClick={save}>Save</button>

// ❌ Don't: clickable div — no Tab, no Enter
<div onClick={save}>Save</div>
```

Always preserve a visible focus ring. Don't kill it.

```tsx
// ✅
<button className="focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring">

// ❌ Don't
<button className="focus:outline-none"> {/* removes ring with no replacement */}
```

## Semantic HTML first

Reach for ARIA only when a native element doesn't exist.

| Need | Use | Not |
|------|-----|-----|
| Action | `<button>` | `<div onClick>` |
| Navigation | `<a href>` | `<button onClick={navigate}>` |
| Section landmark | `<nav>`, `<main>`, `<aside>`, `<footer>` | `<div className="nav">` |
| Heading | `<h1>` → `<h6>` in order | `<div className="text-2xl">` |
| List | `<ul>` / `<ol>` / `<li>` | `<div>`s with bullets |

Heading order: never skip levels. One `<h1>` per page.

## ARIA: first rule is don't

Use ARIA only when no native semantic exists. Wrong ARIA is worse than no ARIA.

```tsx
// ❌ Don't: redundant role
<button role="button">

// ❌ Don't: aria-label duplicating visible text
<button aria-label="Save">Save</button>

// ❌ Don't: aria-hidden on focusable element
<button aria-hidden="true">Save</button>

// ✅ aria-label when there's no visible text
<button aria-label="Close dialog"><XIcon /></button>

// ✅ aria-describedby to associate help/error text
<input aria-describedby="email-help" />
<p id="email-help">We'll never share your email.</p>
```

## Color contrast

| Text | Minimum ratio |
|------|---------------|
| Body (< 18px / < 14px bold) | 4.5:1 |
| Large (≥ 18px / ≥ 14px bold) | 3:1 |
| UI components, graphics | 3:1 |

Use semantic token pairs from the Design Tokens section above — they're pre-verified. **Don't pair raw colors without checking.**

```tsx
// ✅ Verified pair
<div className="bg-background text-foreground">

// ❌ May fail contrast in light or dark
<div className="bg-slate-300 text-slate-500">
```

Verified pairs: <!-- TODO: list pairs that pass in both light and dark mode -->

## Forms

```tsx
// ✅ Bound label
<label htmlFor="email">Email</label>
<input id="email" type="email" required aria-required="true" />

// ✅ Error tied to field
<input
  id="email"
  aria-invalid={!!error}
  aria-describedby={error ? "email-error" : undefined}
/>
{error && <p id="email-error" className="text-destructive">{error}</p>}

// ❌ Don't: placeholder as label
<input placeholder="Email" />

// ❌ Don't: red border alone for error (color-only signal)
<input className={cn(error && "border-red-500")} />
```

Required fields: mark **both** visually (`*` or "Required") **and** programmatically (`aria-required` or `required`).

## Motion

Respect `prefers-reduced-motion`. Disable or shorten non-essential animations.

```tsx
// ✅ Tailwind motion-safe / motion-reduce
<div className="motion-safe:animate-fade-in motion-reduce:animate-none">
```

Avoid: auto-playing video, parallax, scroll-jacking, anything flashing > 3× per second.

## Images

```tsx
// ✅ Content image — describe it
<Image src="..." alt="Sunset over Borobudur temple" />

// ✅ Decorative — empty alt
<Image src="..." alt="" />

// ❌ Don't: omit alt entirely
<Image src="..." />

// ❌ Don't: filename or "image of..."
<Image src="..." alt="IMG_2034.jpg" />
<Image src="..." alt="image of a temple" />
```

## Quick check before merging UI

- [ ] Tab through the feature — every interactive thing reachable, focus visible
- [ ] Run with a screen reader once (VoiceOver: Cmd+F5)
- [ ] Resize to 320px width — no horizontal scroll, no clipped content
- [ ] Toggle dark mode — contrast still passes
- [ ] OS reduced-motion on — animations don't break the UI
