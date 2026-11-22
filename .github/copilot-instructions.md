# Copilot instructions

> Repository instructions for GitHub Copilot (chat, agent mode, cloud agent,
> code review, CLI). Keep this file short and true (~100 lines). If an
> instruction is wrong, fix it in the same PR that finds the problem. Detail
> lives in `docs/` and is read when needed. Path-specific rules live in
> `.github/instructions/`.

## Project

- **Name:** {{PROJECT_NAME}}
- **What it does:** {{ONE_LINE_DESCRIPTION}}
- **Stack:** {{e.g. TypeScript / Node 22, Python 3.12, Go 1.23}}
- **Entry points:** {{e.g. `src/index.ts`, `app/main.py`}}

## Commands

All commands are non-interactive and safe to run from a fresh shell.

| Purpose | Command |
|---------|---------|
| Install deps | `{{e.g. npm ci  |  uv sync}}` |
| **Verify everything** (lint, types, tests) | `bash scripts/verify.sh` |
| Run one test | `{{e.g. npx vitest run path/to/file.test.ts}}` |
| Run the app locally | `{{e.g. npm run dev}}` |

`scripts/verify.sh` is the definition of done. It must pass before you say a
task is finished or open a PR. It prints only failures, so read all of its output.

## Repo map

```
{{fill in — keep to ~10 lines}}
src/            application code
tests/          tests mirror src/ layout
docs/specs/     one spec per feature (write before coding non-trivial work)
docs/decisions/ ADRs — why things are the way they are
scripts/        verify.sh and other automation
PROGRESS.md     running handoff log between sessions
```

## How to work here

1. **Read `PROGRESS.md` first** (latest entry at the top) and the spec it points to.
2. **Non-trivial change? Spec first** with the `spec` skill. Get it agreed before coding.
3. **Small steps.** One change, then `bash scripts/verify.sh`, then the next.
   Don't stack unverified edits.
4. **Tests prove behavior.** New behavior gets a test; bug fixes get a
   regression test that fails before the fix.
5. **Stop and ask** (don't guess) when requirements are ambiguous, an action
   is destructive or irreversible, or you'd need a new dependency or secret.
   As the cloud agent, ask in the PR instead of guessing.
6. **Before you finish:** use the `review` skill (or the `reviewer` agent) for
   non-trivial diffs, then the `handoff` skill to update `PROGRESS.md`.

## Skills and agents

| Name | Where | When |
|------|-------|------|
| `spec` | `.github/skills/spec` | Before any change you couldn't describe in one commit message |
| `verify` | `.github/skills/verify` | Run verify and fix failures one at a time |
| `review` | `.github/skills/review` | Before a PR: fresh-context review against the spec |
| `handoff` | `.github/skills/handoff` | End of session: write the `PROGRESS.md` entry |
| `reviewer` agent | `.github/agents/reviewer.agent.md` | Pick it in the agent picker for a read-only review |

## Conventions

- Commits: Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`, `chore:`).
- Branches: `feat/<slug>`, `fix/<slug>`. Never commit directly to `main`.
- Prefer editing existing files over new ones; small files (< 400 lines) and
  small functions (< 50 lines).
- Immutable data and explicit error handling; no silent `catch {}`.
- {{project-specific conventions: naming, error types, logging, etc.}}

## Boundaries

- **Never** commit secrets, `.env` files, or credentials. Use `.env.example`.
- **Never** run destructive commands (`rm -rf`, `git push --force`, DB drops,
  `terraform apply`) without explicit human approval.
- **Never** disable or delete a failing test to make verify pass.
- Do not edit generated files: {{list, e.g. `dist/`, `*.gen.ts`}}.

## Code review focus

When reviewing a PR: check it against its spec's acceptance criteria first,
then correctness, security, and missing tests. Skip style nits that a linter
would catch.

## Gotchas

<!-- Add things that bit Copilot (or a human) so it doesn't happen twice. -->
- {{e.g. "Tests need `TZ=UTC` or date tests fail."}}

## More context (read when needed)

- `docs/agents/PRINCIPLES.md` — the engineering principles this repo follows
- `docs/agents/WORKFLOW.md` — spec → implement → verify → review → handoff
- `docs/decisions/` — architecture decision records
