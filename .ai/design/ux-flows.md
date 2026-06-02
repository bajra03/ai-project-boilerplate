# UX Flows & Information Architecture

The map of what screens exist, how users move between them, and what each happy
path looks like. Filled during design-discovery (see `../prompts/design-discovery.md`).
The roadmap's Phase 1 deliverables are derived from these flows — keep them honest.

See also: `design.md` (§ Component State Catalog — the per-screen states),
`../roadmap.md` (phases built from these flows), `../product/brief.md` (who/why).

## Information architecture

Top-level navigation and where things live. Keep it shallow — depth hides features.

```
<!-- TODO: replace with the real nav map -->
/
├── /tours              # browse (public)
├── /tours/[id]         # detail + book
├── /bookings           # my bookings (auth)
├── /dashboard          # operator home (auth, role: operator)
└── /settings
```

- **Primary nav:** <!-- TODO: the 3–5 destinations always reachable -->
- **Auth boundary:** <!-- TODO: which routes require login / which roles -->

## Core flows

Document the **3–5 flows that define the product** (the ones in the roadmap MVP).
For each: the goal, the happy-path steps, and the off-path states. Keep it to
steps, not wireframes.

### Flow: <!-- TODO: e.g. "Book a tour" -->

- **Actor:** <!-- TODO: who (guest / member / operator) -->
- **Goal:** <!-- TODO: the one thing they're trying to accomplish -->
- **Entry:** <!-- TODO: where they start (search, deep link, email) -->

```
<!-- TODO: happy path as numbered steps + screen each lands on -->
1. Browse /tours              → card grid
2. Open /tours/[id]           → detail
3. Pick date + seats          → booking form (inline validation)
4. Confirm + pay              → checkout
5. See confirmation           → /bookings/[id] + email receipt
```

- **Success:** <!-- TODO: what "done" looks like to the user -->
- **States per screen** (wire to `design.md` § state catalog):
  - Empty: <!-- TODO: e.g. no tours match the filter -->
  - Loading: <!-- TODO: skeletons for the list/detail -->
  - Error: <!-- TODO: payment declined, sold out mid-flow -->
  - Edge: <!-- TODO: not logged in at step 4, seats taken between view + book -->

### Flow: <!-- TODO: second core flow -->

<!-- TODO: repeat the structure above -->

## Cross-cutting states

States every flow must handle, so AI doesn't ship only the happy path:

- **Unauthenticated** hitting an auth route → <!-- TODO: redirect to login + return -->
- **Unauthorized** (wrong role) → <!-- TODO: 404 vs 403 (see ../patterns/security.md) -->
- **Offline / network error** → <!-- TODO: retry affordance -->
- **First run / zero data** → <!-- TODO: onboarding or empty state with primary action -->

## Notes for AI

- Don't add a screen that isn't in the IA above without flagging it.
- Every new flow gets its four states (empty/loading/error/edge) before "done".
- Keep nav shallow; if a flow needs 5+ steps, question the flow.
