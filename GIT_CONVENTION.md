# Git Convention

Use this convention from day one. It works for solo projects and teams.

## Branch Model

- `main`: the only long-lived branch. Protect it.
- short-lived branches: branch from `main`, merge through PR, delete after merge.

There is no `develop` branch. Normal work goes on a short-lived branch and merges
straight into `main`.

## Branch Naming

Use:

```text
<type>/<ticket-id>-<short-description>
```

If there is no ticket ID:

```text
<type>/<short-description>
```

Allowed branch types:

- `feat/` for user-facing features
- `fix/` for bugs
- `refactor/` for restructuring without behavior changes
- `chore/` for maintenance, dependencies, and config
- `docs/` for documentation
- `ci/` for CI/CD changes
- `test/` for tests
- `perf/` for performance work
- `release/` for release preparation
- `hotfix/` for production fixes
- `experiment/` for disposable spikes

Rules:

- lowercase only
- hyphenate words
- keep names under 50 characters when practical
- always branch from `main`

## Commit Convention

Use Conventional Commits:

```text
<type>(<scope>): <subject>
```

Allowed commit types:

- `feat`
- `fix`
- `docs`
- `style`
- `refactor`
- `perf`
- `test`
- `chore`
- `ci`
- `revert`

Subject rules:

- imperative mood: `add`, not `added`
- lowercase first letter
- no trailing period
- max 72 characters for the first line

Examples:

```text
feat(network): add UDP cursor stream
fix: handle zero-length drag gesture
chore(deps): update swift-navigation
```

## Pull Requests

- Open all PRs into `main`.
- Keep PRs reviewable. Split large work into stacked branches.
- Use a PR title that follows the commit convention.

## Git Hooks

This repo can install shared hooks from `.githooks/`:

```bash
git config core.hooksPath .githooks
```

Included hooks:

- `commit-msg`: validates Conventional Commit first lines.
- `pre-commit`: runs staged whitespace checks.
- `pre-push`: blocks direct pushes to protected branches.

## Setup Checklist

```bash
git config core.hooksPath .githooks
```
