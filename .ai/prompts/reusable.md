# Reusable Prompts

Prompts you find yourself reaching for repeatedly. Copy-paste, fill blanks.

## Fill `.ai/` for a new project

One-time setup after copying the boilerplate. Use the structured prompt in `./bootstrap.md` — it walks Claude through scan → ask → fill → first ADR → cleanup.

## Add a feature

```
Add <feature> following .ai/runbooks/add-feature.md.
Match patterns in .ai/patterns/*.
Don't introduce new dependencies without asking.
Show me a step-by-step plan first, before writing code.
```

## Debug

```
Help me debug <symptom>. Reproduce: <steps>.
Follow .ai/runbooks/debug.md — form a hypothesis before changing code.
Don't fix at the symptom; find the root cause.
```

## Refactor

```
Refactor <code> to <goal>. Don't change behavior.
Run typecheck + tests after each step. Stop and show me if any test fails.
```

## Code review

```
Review the diff. Focus on:
1. Does it match patterns in .ai/patterns/?
2. Are there hidden assumptions / unhandled edge cases?
3. Is anything in CLAUDE.md "Never" being violated?

Be specific — file:line for each comment.
```

## Update docs

```
I just shipped <change>. Update .ai/ to reflect it:
- schemas/database.md if tables changed
- schemas/api.md if endpoints changed
- patterns/ if a new pattern emerged
- tasks/current.md to mark done

Don't update files that didn't actually change.
```

## Custom

<!-- TODO: add prompts you reuse in this project -->
