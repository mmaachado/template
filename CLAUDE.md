# CLAUDE.md

## Project

Describe the scope of your project. Complete specification at `.claude/specs/` (start by `.claude/specs/README.md`).

## Stack (adapt at your needs)

- Django / Flask / FastAPI - Python 3.13.14 - uv (packaging)
- Dev tools (`black`, `ruff`, `pytest`, `taskipy`) stay as dev-deps at `pyproject.toml` — never mixed with runtime dependencies.

## Architecture (adapt at your needs)

- Layered (n-tier): presentation (views) → application (services) → domain (models) → infrastructure (integrations). Details in `.claude/specs/`
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

## Automation (.claude)

- Agents: `code-reviewer` (before commit), `test-agent` (run/fix tests).
- Reusable skills stay at `.claude/skills/`.

## Versioning

- SemVer (`MAJOR.MINOR.PATCH`), declared once in `[project].version` within
  `pyproject.toml`.
- Every release records changes under `## [Unreleased]` in `CHANGELOG.md` (Keep a Changelog).
  Version bumps and `vX.Y.Z` tags are handled by the developer — process outlined in `CONTRIBUTING.md`.
