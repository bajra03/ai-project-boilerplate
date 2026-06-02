# Testing Patterns

How we test in this project. Match this style — the goal is confidence per
minute, not coverage theater. Test behavior users depend on; skip tests that
just restate the implementation.

See also: `error-handling.md` (what errors to assert), `api-routes.md` (route
contracts), `db-queries.md` (what to hit a real DB for).

## The pyramid (what to write, how much)

| Layer | Tests what | Speed | Bulk of suite? |
|-------|-----------|-------|----------------|
| Unit | Pure functions, helpers, reducers, validation | ms | Yes — most tests |
| Integration | Route handler + real DB, query modules, services | 10–100ms | Some — the critical paths |
| E2E | Full user flow through the browser | seconds | Few — happy paths + top risks |

Rule of thumb: push logic *down* into pure functions so it's unit-testable;
reserve slow E2E for flows that break revenue or auth.

## What to mock vs run real

```ts
// ✅ Run REAL: your own DB (test database), your own query modules, pure logic
// ✅ MOCK: third-party network (Stripe, email, LLM provider), clock, randomness

// ✅ Mock the external boundary only
vi.mock("@/lib/stripe", () => ({ charge: vi.fn().mockResolvedValue({ id: "ch_test" }) }))

// ❌ Don't: mock your own query module in an integration test
vi.mock("@/db/queries/tours")   // now you're testing the mock, not the query
```

- Mock at the **edge of your system** (HTTP clients, SDKs), never your own internals.
- Use a **real test database** for integration tests — truncate between tests.
- Freeze time/uuid where output depends on them, so assertions are stable.

## Location & naming

```
src/lib/price.ts
src/lib/price.test.ts          # co-located unit test
src/app/api/tours/route.ts
src/app/api/tours/route.test.ts # integration test next to the handler
e2e/booking.spec.ts            # E2E lives in /e2e, named by user flow
```

- Co-locate unit/integration tests as `<name>.test.ts(x)`.
- Name the test by the **behavior**, not the function:
  `it("rejects a booking when no seats remain")` not `it("createBooking works")`.

## Fixtures & factories

Build test data with factories that take overrides — never hand-roll full
objects in every test.

```ts
// test/factories.ts
export const makeTour = (over: Partial<NewTour> = {}): NewTour => ({
  name: "Test Tour", operatorId: "op_1", availableSeats: 10, ...over,
})

// in a test — only state what matters for THIS test
const tour = makeTour({ availableSeats: 0 })
```

## Anatomy of a good test

```ts
it("rejects a booking when no seats remain", async () => {
  // Arrange — minimal setup, only what the assertion needs
  const tour = await seedTour(makeTour({ availableSeats: 0 }))
  // Act
  const res = await POST(req({ tourId: tour.id }))
  // Assert — on observable behavior (status, body), not internals
  expect(res.status).toBe(409)
  expect(await res.json()).toMatchObject({ error: { code: "sold_out" } })
})
```

## Negative examples

```ts
// ❌ Don't: assert on implementation details
expect(createBooking).toHaveBeenCalledTimes(1)   // brittle; refactor breaks it

// ❌ Don't: one giant test covering five behaviors
it("booking flow", async () => { /* 80 lines, 12 asserts */ })

// ❌ Don't: share mutable state between tests (order-dependent, flaky)
let user            // leaks across tests
beforeAll(() => { user = makeUser() })

// ❌ Don't: test the framework or the library — test YOUR logic
it("zod parses a string", () => {})   // not your code
```

## Coverage philosophy

Coverage is a **smell detector, not a target.** Chasing 100% rewards testing
trivial getters and punishes nothing. Instead:

- Every bug fix ships with a regression test that fails without the fix.
- Every API route has at least one integration test for its success + main error.
- Don't gate merges on a coverage number — gate on "are the risky paths tested?".

## Project specifics

- **Runner:** <!-- TODO: Vitest / Jest — and the command, e.g. `pnpm test` -->
- **E2E:** <!-- TODO: Playwright / Cypress — command, e.g. `pnpm test:e2e` -->
- **Test DB setup:** <!-- TODO: how the test database is provisioned + reset -->
- **What we deliberately don't test:** <!-- TODO: e.g. generated code, third-party UI -->
