# CLAUDE.md

## Project

Describe the scope of your project. Complete specification at `.claude/SPEC.md`.

## Stack (adapt at your needs)

- Django / Flask / FastAPI - Python 3.13.14 - uv (packaging)
- Dev tools (`black`, `ruff`, `pytest`, `taskipy`) stay as dev-deps at `pyproject.toml` — never mixed with runtime dependencies.

## Architecture (adapt at your needs)

- Layered (n-tier): presentation (views) → application (services) → domain (models) → infrastructure (integrations). Details in `.claude/SPEC.md`
- Directories: `apps/`, `integrations/`. Views do not call API's or contain business logic.

## UI (adapt at your needs)

- Reusable components from the `django-shadcn` package, built on `django-cotton` (`<c-*>` tags) and
  `django-tailwind-cli`.
- Self-hosted: no assets are loaded from a CDN and there is no Node dependency in the build process — CSS, JS, and fonts are versioned or compiled using Tailwind CLI.

## Coding rules (mandatory)

- PEP 8: line length 79, double quotes (normalized by `black`), imports sorted by
  `ruff` (rule `I`). `black` handles formatting, `ruff` handles static analysis — `task format` and
  `task lint`; `task test` runs both before the test suite.
- Codebase in English;
- No referencing documentation within the code: no comments, docstrings, or strings citing specs,
  `CLAUDE.md`, or requirements, nor using markers (`RF-x`, `RNF-x`, `§`, "per spec", document numbers).
  The code must be self-explanatory; traceability belongs in the specs.
- Secrets via environment variables — never hardcoded or committed to git.
- YAGNI: no speculative abstraction; do not reimplement stdlib/lib functionality.

## Tests (TDD)

- Every new or modified module starts with tests (Red → Green → Refactor). Run via `task test`.

## Logging

- Mandatory, clear informational logs in the terminal and log file. Never log secrets; never use `print`.

## Workflow (project lifecycle)

1. **Setup** — clone/create the repo (`.github/CONTRIBUTING.md` → Getting started).
2. **Plan** — open Claude Code in Plan Mode, `Opus`. Developer describes the
   full scope of the project/feature to build.
3. **Architect & close the spec** — distill the requirements fully and close
   `.claude/SPEC.md`: scope/deliverable, architecture decisions, lint/format
   standards, and the split of what's the agent's job vs. the developer's
   job. Prefer `improve-codebase-architecture` and `verification-planning`
   for this. `caveman` is for the conversational replies to the developer
   only — never for `SPEC.md` itself, which must stay maximally detailed: a
   developer reading it later has to understand why, how, and for what
   purpose each decision exists.
4. **Branch** — create/checkout the working branch from `master`.
5. **Build** — call `development-agent` (`Sonnet`) → `test-agent`
   (`Sonnet`) → `code-reviewer` (`Opus`), in that order. Use `caveman` +
   `ponytail` for terse, lazy-first execution, and `karpathy-coder` to keep
   the diff surgical. Any decision an agent can't make from
   `SPEC.md`/`CLAUDE.md` alone stops it — it escalates to the
   session-starting agent to decide with the developer.
6. **Security gate** — development done, run `security-audit`. Findings
   route back to the session-starting agent to orchestrate the action plan
   with the developer — same escalation rule as step 5.
7. **Commit** — the agent commits per feature implemented in `SPEC.md`,
   message per `.github/CONTRIBUTING.md`'s convention
   (`category(scope): message`, e.g.
   `feat(components): add the switch component`), signed with
   `Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>`. PR and merge
   are always the developer's.
8. **Release** — after merge, bump the version tag if applicable
   (`.github/CONTRIBUTING.md` → Releases).
9. **Document** — record the implementation/changes as agreed during
   planning (`CHANGELOG.md` under `## Unreleased`, plus anything `SPEC.md`
   assigned to documentation). Run `self-improving-agent`
   (`/si:memory-review` and, if it flags a candidate, `/si:promote`) so
   what was learned in the delivery graduates into `CLAUDE.md`/
   `.claude/rules/` instead of staying stuck in auto-memory.
10. **Next delivery** — back to step 2 for the next scope.

## Automation (.claude)

- Agents: `development-agent` (code from `SPEC.md`), `test-agent` (run/fix
  tests), `code-reviewer` (final review before commit).
- Skills used along the workflow above: `improve-codebase-architecture` +
  `verification-planning` (planning), `caveman` (chat replies only) +
  `ponytail` + `karpathy-coder` (build), `security-audit` (pre-commit gate).
- Reusable skills stay at `.claude/skills/`.

## Versioning

- SemVer (`MAJOR.MINOR.PATCH`), declared once in `[project].version` within
  `pyproject.toml`.
- Every release records changes under `## [Unreleased]` in `CHANGELOG.md` (Keep a Changelog).
  Version bumps and `vX.Y.Z` tags are handled by the developer — process outlined in `.github/CONTRIBUTING.md`.
