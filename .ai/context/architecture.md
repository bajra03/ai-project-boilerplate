# Architecture

## System overview

<!-- TODO: 3–5 sentence high-level summary. What does the system do? Who are its users? What are the major moving parts? -->

## Diagram

```
<!-- TODO: ASCII diagram of major components. Example:

[Browser] ──> [Next.js App] ──> [Postgres]
                   │
                   ├──> [Stripe]
                   ├──> [Clerk Auth]
                   └──> [Resend Email]
-->
```

## Major components

<!-- TODO: list each major part + 1-line purpose -->

- **Web app** — <!-- where users interact -->
- **Admin app** — <!-- internal CMS -->
- **API** — <!-- shared backend / route handlers -->
- **DB** — <!-- persistent state -->
- **Queue / jobs** — <!-- async work, if any -->
- **External services** — <!-- payments, email, auth, storage -->

## Data flow — golden path

<!-- TODO: walk through the most important user action end-to-end. Example:
1. User clicks "Book tour"
2. Client validates with Zod
3. POST /api/bookings hits route handler
4. Handler verifies auth (Clerk) and availability (DB)
5. Stripe checkout session created
6. On webhook success: booking row inserted, email queued
7. User redirected to confirmation page
-->

## Boundaries

<!-- TODO: what crosses process / network boundaries? Where do failures matter? -->

- **Trust boundary:** <!-- e.g., user input vs internal -->
- **Network boundary:** <!-- which services can fail independently -->
- **Transaction boundary:** <!-- where atomicity matters -->

## Out of scope

<!-- TODO: things that look like they belong but don't. Saves AI from suggesting them. -->
