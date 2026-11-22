# copilot-repo-template (GitHub Copilot)

A GitHub template repository that makes any new repo **ready for GitHub
Copilot on day one**: VS Code agent mode, the Copilot cloud agent, Copilot code
review, and the Copilot CLI. Language-agnostic: works for Node, Python, Go,
Rust, or anything you plug into `scripts/verify.sh`.

Using Claude Code instead? Use
[repo-template](https://github.com/joshoohaah/repo-template).

> This README is replaced by a project README when you run `scripts/init-template.sh`.

## What's inside

| Path | Purpose | Idea from |
|------|---------|-----------|
| `.github/copilot-instructions.md` | Repo instructions every Copilot surface reads: commands, repo map, boundaries, gotchas, review focus | [AGENTS.md](https://github.com/agentsmd/agents.md) |
| `.github/instructions/tests.instructions.md` | Extra rules applied only to test files (`applyTo` glob) | — |
| `.github/skills/` | Agent Skills: `spec`, `verify`, `review`, `handoff` | Spec Kit, harness engineering |
| `.github/agents/reviewer.agent.md` | Read-only **reviewer custom agent**: checks a diff against its spec | harness engineering (independent review) |
| `.github/workflows/copilot-setup-steps.yml` | Installs your toolchain before the **cloud agent** starts, so it can run `verify.sh` | — |
| `.vscode/settings.json` | Agent mode auto-approves verify + git-read; never auto-runs `rm` or force-push | — |
| `scripts/verify.sh` | Single non-interactive "is it working?" command; auto-detects stack; prints only failures | [mini-SWE-agent](https://github.com/SWE-agent/mini-swe-agent), [12-factor](https://github.com/humanlayer/12-factor-agents) #9 |
| `PROGRESS.md` | Append-only handoff log so sessions resume cold | [harness engineering](https://github.com/walkinglabs/learn-harness-engineering) (continuity) |
| `.github/workflows/ci.yml` | Runs the same `verify.sh` | — |
| `docs/specs/`, `docs/decisions/` | Spec-before-code template; ADRs | [Spec Kit](https://github.com/github/spec-kit) (lite) |
| `docs/agents/PRINCIPLES.md` | The principles above, applied to this repo | all |
| `.github/` (rest) | PR template with verification checklist, `agent-task` issue form for the cloud agent, Dependabot | 12-factor #11 |

## Use it

```bash
gh repo create <you>/my-app --private --template <you>/copilot-repo-template --clone
cd my-app
bash scripts/init-template.sh "my-app" "One-line description of what it does"
```

Then in Copilot agent mode: *"Fill in the `{{...}}` placeholders in
.github/copilot-instructions.md from the code, and add the right toolchain to
ci.yml and copilot-setup-steps.yml."*

**Existing repo?** Copy `.github/copilot-instructions.md`, `.github/instructions/`,
`.github/skills/`, `.github/agents/`, `.github/workflows/`, `.vscode/settings.json`,
`PROGRESS.md`, `scripts/`, and `docs/`.

## Daily loop

| Where | Loop |
|-------|------|
| VS Code agent mode | read `PROGRESS.md` → `spec` skill → small steps + `verify` → new chat with `reviewer` agent → `handoff` skill → PR |
| Cloud agent | open an `agent-task` issue → assign it to Copilot → it opens a PR → request Copilot code review → you review |

Details: [docs/agents/WORKFLOW.md](docs/agents/WORKFLOW.md).

## Customizing verify

`scripts/verify.sh` runs `lint` / `typecheck` / `test` / `build` npm scripts if
present, `ruff`/`mypy`/`pytest` for Python, `go vet`/`go test`, and
`cargo fmt`/`clippy`/`test`. Add anything else in `scripts/verify.local.sh`:

```bash
# scripts/verify.local.sh — sourced by verify.sh
run_step "docker compose config" docker compose config -q
```

On Windows without a bash prompt: `pwsh scripts/verify.ps1`.

## Not included (on purpose)

- **MCP servers** — add `.vscode/mcp.json` per project when one is needed.
- **Agent evals ([Harbor](https://github.com/harbor-framework/harbor))** — add
  when a repo *is* an agent or you're tuning prompts/config and need evidence.
