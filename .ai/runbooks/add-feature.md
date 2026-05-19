# Runbook: Add a Feature

Step-by-step for adding a new feature end-to-end. Follow in order.

## 0. Understand

- Read `.ai/context/architecture.md` to find where this fits
- Check `.ai/tasks/current.md` and `backlog.md` to confirm scope
- If unclear, **ask** before writing code

## 1. Data layer

1. Add / modify table in `src/db/schema.ts`
2. Generate migration: `pnpm db:generate`
3. Apply: `pnpm db:migrate`
4. Update `.ai/schemas/database.md` and `types.md`
5. Add query functions in `src/db/queries/<resource>.ts` — see `.ai/patterns/db-queries.md`

## 2. API layer

1. Add route in `src/app/api/<resource>/route.ts` — see `.ai/patterns/api-routes.md`
2. Zod schema for request body
3. Auth check at top of handler
4. Call query function, return via `apiOk` / `apiError`
5. Update `.ai/schemas/api.md` with the new endpoint

## 3. UI layer

1. Add component(s) in `src/components/<feature>/` — see `.ai/patterns/components.md`
2. Add page in `src/app/<route>/page.tsx`
3. Forms: react-hook-form + same Zod schema as API (export schema, reuse on both sides)
4. Loading + error states (use `loading.tsx`, `error.tsx`)

## 4. Tests

1. Unit: query functions + pure utils
2. Integration: API route with real DB (test schema)
3. E2E (optional, for critical paths): Playwright

## 5. Verify

```bash
pnpm typecheck
pnpm lint
pnpm test
pnpm build         # catches Server/Client Component mistakes
```

## 6. Document

- Add ADR if this introduced an architectural choice
- Update `.ai/tasks/current.md` (mark done, move next item up)
- Update glossary if new domain terms emerged

## Common mistakes to avoid

- ❌ Skipping migration regenerate after schema edit
- ❌ Validating in two places with two different schemas (duplicate Zod)
- ❌ Querying DB directly from a component (always go through `queries/`)
- ❌ Forgetting `error.tsx` / `loading.tsx`
- ❌ Adding a feature without updating `.ai/` docs (silent context rot)
