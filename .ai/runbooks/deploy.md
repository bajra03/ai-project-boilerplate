# Runbook: Deploy

How to ship a change to production. Update with project-specific details.

## Environments

| Env | URL | Branch | Auto-deploy? |
|-----|-----|--------|--------------|
| Local | localhost:3000 | any | n/a |
| Preview | `<branch>.vercel.app` | PR | yes |
| Staging | <!-- TODO --> | `staging` | yes |
| Production | <!-- TODO --> | `main` | <!-- yes/no --> |

## Pre-deploy checklist

- [ ] Typecheck passes: `pnpm typecheck`
- [ ] Tests pass: `pnpm test`
- [ ] Build succeeds locally: `pnpm build`
- [ ] Migrations applied to staging — confirm with `pnpm db:status`
- [ ] Env vars added in hosting dashboard (compare against `.env.example`)
- [ ] Feature flag set correctly if gated

## Deploying

<!-- TODO: project-specific deploy commands. Example for Vercel: -->

1. Merge PR to `main`
2. Vercel auto-builds and deploys
3. Confirm health check: <!-- TODO: health endpoint URL -->
4. Smoke-test the changed flow in prod

## Rollback

<!-- TODO: how to roll back. Vercel: redeploy previous build from dashboard. Custom: ... -->

## Migrations

- **Additive migrations** (new column nullable, new table) — safe to deploy before code that uses them
- **Destructive migrations** (drop column, rename) — deploy code first that ignores the column, then drop in a separate migration
- **Never** edit a migration that's been applied to staging or prod

## Post-deploy

- Watch error rates for 15 min (Sentry / logs)
- Watch latency / 5xx in <!-- TODO: dashboard URL -->
- Update `.ai/tasks/current.md` — mark shipped
