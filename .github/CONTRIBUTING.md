# Contributing

Thanks for your interest in contributing to <project-name>.

Please take a moment to read this before opening your first pull request, and
check the open issues and pull requests to see if someone is already working on
something similar.

## About this repository

Provide a resume about your project.

## Structure

```
.
├── .claude/             Claude Code guidelines & skills
├── .github/             GitHub CI/CD configurations
│   ├── ISSUE_TEMPLATE/
│   ├── workflows/
│   ├── CONTRIBUTING.md
│   ├── dependabot.yml
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── SECURITY.md
│
├── CHANGELOG.md        Significant modifications into this project
├── LICENSE
├── .python-version
├── pyproject.toml
├── README.md
└── CLAUDE.md           Claude reference file
```

## Getting started

Fork the repository, then:

```bash
git clone https://github.com/your-username/template.git
cd template
git checkout -b my-new-branch
uv sync
```

## Tests and linting

```bash
uv run task lint     # ruff check
uv run task format   # black format
uv run task test     # ruff check, then pytest with coverage
```

Code is formatted by black at 79 columns with double quotes. Please make sure
tests pass before opening a pull request, and add tests for new behaviour.

## Commit convention

Please write commit messages as `category(scope): message`, using one of:

`feat`, `fix`, `refactor`, `docs`, `build`, `test`, `ci`, `chore`.

For example: `feat(components): add the switch component`.

## Releases

Maintainers only. The version is never written in a file — it comes from the git
tag, so cutting a release means creating one:

```bash
git checkout master && git pull
git tag -a v1.0.0 -m "v1.0.0"
git push origin v1.0.0
```

Add the section to `CHANGELOG.md` first, headed `## <version>`. The workflow
copies it into the GitHub release and appends the generated commit list below
it. Without a matching section the release still goes out, carrying only the
generated notes — which is why anything a reader has to know before upgrading
belongs in the changelog and not in a commit message.
