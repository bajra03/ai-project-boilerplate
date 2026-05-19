# Runbook: Debug

Step-by-step for diagnosing a bug. Don't skip to fixes — get to root cause first.

## 0. Reproduce

- Get a **deterministic** repro before doing anything else
- If flaky, find the minimum reliable trigger
- Note the exact env: branch, env vars, browser, user role

## 1. Isolate

- Bisect: what's the smallest change that toggles the bug?
- Check `git log` for nearby changes
- If recent: `git bisect` between last-known-good and current

## 2. Inspect the layer

| Layer | Tool |
|-------|------|
| Browser UI | DevTools, React DevTools |
| Network | Network tab, server logs |
| API | Logs in route handler, request payload |
| DB | <!-- TODO: psql / Drizzle Studio / Supabase --> |
| External (Stripe etc.) | Provider dashboard logs |

## 3. Form a hypothesis

State the hypothesis **before** changing code:

> "I think X is failing because Y. If true, I'd expect Z."

Test with a single targeted change. If wrong, hypothesis was wrong — don't pile on more changes.

## 4. Fix at the root, not the symptom

- ❌ `if (x === undefined) x = 0` — masks why x became undefined
- ✅ Find where x should be set; ensure it always is

## 5. Add a regression test

If the bug was reachable through a code path users hit, write a test that fails without your fix and passes with it.

## 6. Document if non-obvious

- ADR if it changed a design decision
- Code comment if a future reader would re-introduce the bug

## Anti-patterns

- ❌ `console.log` everywhere, never cleaned up — use the debugger or structured logs
- ❌ Wrapping in try/catch to make the error go away
- ❌ Reverting a commit without understanding why it broke things
- ❌ "It works on my machine" — get the env diff
