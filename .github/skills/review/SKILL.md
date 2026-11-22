---
name: review
description: Independent review of the current diff against its spec, from a fresh context. Use before opening a PR or when asked to review changes.
---

# Independent review

The author grades its own work generously, so the review should come from a
context that has NOT seen the implementation conversation.

1. If you are in VS Code and the `reviewer` custom agent is available, tell the
   user to start a new chat with the `reviewer` agent selected, and stop.
2. Otherwise, review it yourself as strictly as `.github/agents/reviewer.agent.md`
   describes: get `git diff main...HEAD` plus `git diff`, find the spec in
   `docs/specs/`, and check acceptance criteria, correctness, security,
   tests, and conventions, in that order.
3. Report findings ranked by severity with `file:line` and a concrete failure
   scenario. Don't apply fixes until the user says which ones.

On a pull request, you can also request a review from Copilot code review,
which reads `.github/copilot-instructions.md`.
