# Error Handling

How errors flow through the system. Match this style.

## Principles

1. **Throw at the source, catch at the boundary.** Library code throws; route handlers and React error boundaries catch.
2. **Typed errors** for known failure modes. Generic `Error` for unexpected.
3. **No silent swallow.** Don't `catch (e) {}` — at minimum, log.
4. **User-facing errors** never leak stack traces or internal IDs.

## Error classes

```ts
// src/lib/errors.ts
export class AppError extends Error {
  constructor(
    public code: string,
    message: string,
    public status: number = 400,
    public details?: unknown,
  ) {
    super(message)
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string, id: string) {
    super("not_found", `${resource} not found`, 404, { resource, id })
  }
}

export class AuthError extends AppError {
  constructor(msg = "unauthorized") {
    super("unauthorized", msg, 401)
  }
}
```

## In route handlers

```ts
try {
  // ...
} catch (err) {
  if (err instanceof AppError) {
    return apiError(err.status, err.code, err.details)
  }
  // unexpected: log full, return generic
  logger.error("unhandled", { err })
  return apiError(500, "internal_error")
}
```

## In React

- **Server Components:** throw — Next.js renders nearest `error.tsx`.
- **Client Components:** wrap mutations in try/catch, surface via toast.
- **Always have an `error.tsx`** at each route segment that owns user data.

## Negative examples

```ts
// ❌ Don't: catch and ignore
try { await stripe.charges.create(...) } catch (e) {}

// ❌ Don't: leak internals
return Response.json({ error: err.stack }, { status: 500 })

// ❌ Don't: use error codes for control flow when a status field would do
if (err.message === "no rows") { /* ... */ }   // brittle
```

## Logging

<!-- TODO: which logger, where logs go, what to include / redact -->
