# State Management Patterns

Where state lives and who owns it. The default mistake is copying server data
into a global client store — then fighting to keep them in sync. Match this style.

See also: `components.md` (Server vs Client Components, data fetching),
`api-routes.md` (the endpoints client state reads).

## Classify state first

| Kind | Example | Owner |
|------|---------|-------|
| Server state | Tours, bookings, the current user | Server cache (react-query/SWR) — not a store |
| URL state | Filters, tab, page, search query | The URL (searchParams) |
| Form state | Field values, validation, dirty | Form library (react-hook-form) |
| Ephemeral UI state | Modal open, hover, dropdown | Local `useState` in the component |
| Global client state | Theme, sidebar collapsed, toasts | Small store (Zustand/Context) — last resort |

Decision order: **Server? → react-query. Shareable/bookmarkable? → URL.
Form? → form lib. One component? → useState. Truly global UI? → store.**

## Server state is not client state

```ts
// ❌ Don't: fetch then stuff server data into a global store
const tours = await fetch("/api/tours").then(r => r.json())
useStore.setState({ tours })   // now stale, manually invalidated, duplicated

// ✅ Do: let the query cache own it — caching, refetch, invalidation for free
const { data: tours, isLoading } = useQuery({
  queryKey: ["tours", { operatorId }],
  queryFn: () => fetchTours(operatorId),
})
```

- Key queries by their inputs (`["tours", filters]`) so caching is automatic.
- Mutate via `useMutation` + invalidate the affected `queryKey` — don't hand-edit cache.

## Optimistic updates

```ts
const mutation = useMutation({
  mutationFn: bookSeat,
  onMutate: async (vars) => {
    await qc.cancelQueries({ queryKey: ["tour", vars.tourId] })
    const prev = qc.getQueryData(["tour", vars.tourId])
    qc.setQueryData(["tour", vars.tourId], (t) => ({ ...t, seats: t.seats - 1 }))
    return { prev }                                    // rollback context
  },
  onError: (_e, vars, ctx) => qc.setQueryData(["tour", vars.tourId], ctx.prev),
  onSettled: (_d, _e, vars) => qc.invalidateQueries({ queryKey: ["tour", vars.tourId] }),
})
```

Always capture rollback state in `onMutate` and restore it in `onError`.

## URL as state

```tsx
// ✅ Filters in the URL — shareable, survives refresh, back-button works
const params = useSearchParams()
const status = params.get("status") ?? "all"

// ❌ Don't: mirror URL-worthy state into a store and sync by hand
useStore.setState({ status })   // breaks deep links + back button
```

## Negative examples

```ts
// ❌ Don't: one mega-store for everything
useStore() // { user, tours, modalOpen, theme, formDraft, ... }  — re-renders everywhere

// ❌ Don't: Context for high-frequency state (mouse, scroll) — re-renders all consumers
// ✅ Do: local state or a ref; subscribe narrowly if it must be shared

// ❌ Don't: derive-and-store what you can compute at render
const [fullName, setFullName] = useState(first + " " + last)  // just compute it inline
```

## Project specifics

- **Server cache lib:** <!-- TODO: TanStack Query / SWR / RSC + revalidate — and config -->
- **Global store:** <!-- TODO: Zustand / Context / none — what actually lives there -->
- **Where query keys are defined:** <!-- TODO: e.g. src/lib/query-keys.ts -->
