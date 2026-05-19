# `.ai/design/` — Design rules for AI

Lock the visual language before the first component is written. Prevents AI drift: random Tailwind classes, off-palette colors, ad-hoc spacing, missing focus states.

## Files

- **`tokens.md`** — Color, spacing, typography, radius, shadows, breakpoints, dark mode, z-index
- **`components.md`** — How to *use* design-system components (complements `../patterns/components.md`, which is about how to *write* React components)
- **`a11y.md`** — Accessibility baseline (WCAG, keyboard, ARIA, contrast, forms, motion)

## Optional

Backend-only project? Leave these files empty or delete the folder. Keeping them empty is fine — AI will see "no rules" and not invent any.

## See also

- `../patterns/components.md` — React component file/code conventions
- `../context/tech-stack.md` — Confirms styling stack (Tailwind, shadcn, etc.)
