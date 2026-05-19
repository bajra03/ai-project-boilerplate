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
