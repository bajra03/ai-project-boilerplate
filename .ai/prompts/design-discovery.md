# Design Discovery — Brainstorm Before Filling design.md

Run BEFORE filling `.ai/design/design.md` for the first time. Re-run when the product pivots or rebrands.

## Your role

Act as a Senior Product Manager + Senior Frontend Designer. Don't generate the design system instantly — brainstorm direction with the user first, then fill.

## Process

### 1. DETECT category

From PROJECT.One-liner, scanned routes, stack, and any brand hints, bucket the project into one of these (or close):

- SaaS / dashboard / CRM
- Ecommerce / marketplace
- AI tool / chat
- Fintech / banking
- Travel / booking
- Portfolio / personal
- Social / community
- Restaurant / hospitality
- Editorial / media

If unclear, ask: "How would you describe this product in 5 words?"

### 2. SUGGEST 3–5 style directions

Tailor to the detected category. Each suggestion MUST include:

- **Style name**
- **Short description** (1 line)
- **Vibe / mood**
- **UI characteristics** (palette, type, density, motion)
- **Example websites/apps** (3–4 real, well-known)

Style banks per category — starting point, adapt, don't force-fit:

**SaaS / dashboard / CRM**
- Linear Modern (linear.app, vercel.com) — minimal monochrome, sharp type, subtle motion
- Stripe Professional (stripe.com, retool.com) — confident, gradient accents, illustration
- Notion Friendly (notion.so, height.app) — warm, soft shadows, approachable
- Datadog Dense (datadog.com, grafana.com) — info-dense, mono-friendly, tight type

**Ecommerce**
- Minimal Luxury (apple.com, cos.com, nike.com) — whitespace, premium type, large imagery
- Conversion Focused (gymshark.com, shopify storefronts) — bold CTAs, aggressive hierarchy
- Editorial Lifestyle (zara.com, airbnb.com) — storytelling, magazine layout
- Playful Gen-Z (duolingo.com, liquiddeath.com) — vibrant, rounded, playful

**AI tool / chat**
- Anthropic Calm (claude.ai) — neutral, generous whitespace, restrained
- Linear-meets-AI — sharp, dark-first, accent-heavy
- Playful AI — gradients, glow, conversational

**Fintech**
- Trust Bank (wise.com, mercury.com) — restrained, blues, clarity
- Crypto Bold (rainbow.me, phantom.app) — gradient, glassmorphism, dark

**Travel / booking**
- Editorial Travel (airbnb.com, awaytravel.com) — full-bleed imagery, serif accents
- Practical Booking (booking.com, kayak.com) — info-dense, conversion-led

**Portfolio**
- Designer Showcase — bold type, asymmetric, motion
- Developer Minimal — mono type, dark, terse

**Restaurant / hospitality**
- Warm Hospitality — earthy tones, serif, large photos
- Modern Casual — clean, illustration-friendly

### 3. ASK the user

Present as a numbered list, then offer three branches:

- "Which direction resonates? (1/2/3/4)"
- "Or paste a reference URL to anchor on."
- "Or combine — e.g. 'Linear's type + Notion's warmth'."

Wait for the answer. Don't proceed until the user picks.

### 4. RECORD chosen direction

Write to `.ai/design/design.md`'s `## Design Direction` section (near the top). Fill all fields:

- **Style:** chosen name
- **Vibe:** mood in 3–5 words
- **References:** URLs (chosen examples + any user-provided URL)
- **Philosophy:** 1–2 sentences — what the visual identity communicates
- **Interaction:** snappy / smooth / playful / restrained
- **Animation:** minimal / generous / motion-first
- **Layout:** grid-dense / spacious / card-driven / asymmetric
- **Responsive:** mobile-first / desktop-first / app-shell

### 5. FILL the rest of design.md, aligned with direction

Don't rewrite the file — only replace TODO markers.

**TOKENS section**
- Source of truth line: point to actual config (e.g. `tailwind.config.ts` + `src/app/globals.css`)
- Token map: extract semantic color tokens from globals.css CSS vars / theme config. Palette MOOD must match the chosen direction (vibrant for Gen-Z, neutral for Luxury, blues for Trust Bank, etc.)
- Typography families: read font config (next/font, @import, theme.fontFamily). Family SELECTION must match direction (mono + geometric sans for Linear-style; serif + clean sans for editorial)
- Custom radii / shadows / breakpoints / z-scale: only override the template tables if the project actually defines custom values
- Figma/Storybook URL: anchor at top of Tokens section if user provided one

**COMPONENT RULES section**
- Component library line: detect from code (shadcn/ui at `src/components/ui/`, Radix, MUI, Chakra, Mantine, custom)
- Project-specific rules: only fill if observable patterns exist (e.g. "all icons from lucide-react, sizes 16/20/24 only" — only if grep confirms)
- Motion language: matches the direction's Animation field

**ACCESSIBILITY section**
- Verified pairs: list semantic color pairs that pass WCAG in both light and dark (only if you can verify against globals.css)
- Leave the rest of the baseline as-is — it's WCAG-driven, not direction-specific

## What this is NOT

- Don't dictate a single style — suggest, then ask.
- Don't fill design.md before the user picks a direction.
- Don't ignore a reference URL if the user provides one.
- Don't weaken the WCAG baseline to fit a style.

## Considerations when proposing styles

- Trust signals appropriate to category (fintech > fashion in restraint)
- Conversion vs editorial intent
- Retention via mood (calm tools vs energetic Gen-Z)
- Visual hierarchy and UX clarity
- Mobile responsiveness baked in, not bolted on
