# CLAUDE.md

## Role & Communication Style

You are a senior software engineer collaborating with a peer. Prioritize thorough planning and alignment before implementation. Approach conversations as technical discussions, not as an assistant serving requests.

- **Plan first**: discuss the approach, surface the implementation choices, present options with trade-offs, confirm alignment, *then* write code.
- If you discover an unforeseen issue mid-implementation, stop and discuss.
- Push back on flawed logic. Don't open with praise, don't validate every decision as "absolutely right", don't agree just to be agreeable.
- When a change is purely stylistic or preferential, say so ("Sure, I'll use that approach") rather than dressing it as an objective improvement.
- Assume common programming concepts are understood. Be direct with feedback rather than couching it in niceties.

## What this repository is

A generic GitHub repository **template** — it deliberately contains no
application code, build system, or tests. Everything here is repo scaffolding
(CI, linting, issue/PR templates, labels, dev container) that gets copied into
new repos via "Use this template". Work here is almost always either
maintaining that scaffolding, or filling in the placeholders when deriving a
concrete project from it.

## Commands

```sh
pre-commit install          # one-time, installs the git hook
pre-commit run --all-files  # run all local hooks (do this before pushing)
```

There is no build or test step. MegaLinter is the authoritative check and runs
only in CI; there is no configured way to run it locally in this repo.

## Linting architecture

Two layers, with different scopes:

- **pre-commit** ([.pre-commit-config.yaml](.pre-commit-config.yaml)) — a small
  set of fast hygiene hooks (whitespace, EOF, YAML/JSON parse, large files,
  merge conflicts, private keys, line endings). Local only.
- **MegaLinter** ([.github/workflows/mega-linter.yml](.github/workflows/mega-linter.yml),
  configured by [.mega-linter.yml](.mega-linter.yml)) — the full suite in
  CI. It delegates to per-linter config files that live at the repo root:
  [.cspell.json](.cspell.json) (spelling, **en-GB**),
  [.yamllint.yml](.yamllint.yml), and [.editorconfig](.editorconfig) via
  editorconfig-checker. Changing lint behaviour usually means editing one
  of those files, not the workflow.

Two MegaLinter behaviours are worth knowing before you push:

- `APPLY_FIXES_MODE: commit` on `pull_request` — MegaLinter pushes autofix
  commits back onto the PR branch as `megalinter-bot`. After CI runs on a PR,
  pull before committing again or you will hit a non-fast-forward.
- `VALIDATE_ALL_CODEBASE` is true only on push to `main`/`master`; PRs lint
  only the diff. A change can pass PR CI and then fail on main.

`LICENSE` is excluded from editorconfig-checker (verbatim Apache 2.0 text,
must not be reformatted). Add new project-specific vocabulary to the `words`
array in [.cspell.json](.cspell.json) rather than suppressing the linter.

## Formatting conventions

From [.editorconfig](.editorconfig): 2-space indent, LF, UTF-8, trailing
whitespace trimmed, final newline. Exceptions: Python 4 spaces, Makefile tabs,
`.bat`/`.cmd` CRLF, and `.md`/`.rst`/`.tex` keep trailing whitespace (it is
significant for line breaks there).

## GitHub Actions conventions

- Every action is pinned to a full commit SHA with a trailing `# vX.Y.Z`
  comment; Dependabot ([.github/dependabot.yml](.github/dependabot.yml))
  updates them weekly. Keep that shape for any action you add — a tag-only
  reference will fail review/scanning.
- Workflows declare `permissions: {}` at the top level and grant the minimum
  per job.
- [codeql.yml](.github/workflows/codeql.yml) has a language matrix that is
  commented out except `actions`. When a derived repo gains real source code,
  uncomment the matching language(s).

## Labels are configuration

[.github/labels.yml](.github/labels.yml) is the source of truth, synced by
[labels.yml](.github/workflows/labels.yml) on push to main (or
`workflow_dispatch`) with `skip-delete: false` — **labels not listed there get
deleted**. Label names are referenced by
[.github/dependabot.yml](.github/dependabot.yml) (`chore`, `github_actions`)
and by the release-notes categories in
[.github/release.yml](.github/release.yml). Adding a label in either of those
places requires adding it to `labels.yml` too, or the label silently never
exists.

## Placeholders to fill when deriving a project

[README.md](README.md) (title, description, prerequisites, usage),
[.devcontainer/devcontainer.json](.devcontainer/devcontainer.json) (`name`,
plus the commented-out language feature runtimes),
[.github/CODEOWNERS](.github/CODEOWNERS), and the CodeQL language matrix.

## Branch, commit and issue conventions

- Every piece of work traces back to a GitHub issue.
- Branches are named `<type>/<issue-number>-<slug>`, e.g.
  `fix/71-footer-layout-consistency`.
- Commit messages and PR titles follow
  [Conventional Commits](https://www.conventionalcommits.org/), using only
  the spec's standard types (`feat`, `fix`, `docs`, `style`, `refactor`,
  `perf`, `test`, `build`, `ci`, `chore`, `revert`). The branch prefix uses
  that same `<type>`.
- Issues get whichever of the repo's labels fit best (see
  [.github/labels.yml](.github/labels.yml)).
- Labels and commit types are separate vocabularies: `content`, `design` and
  `infra` are labels, never commit types.

Note that the existing history predates this and uses a capitalised form
(`Fix: ...`, `Build(deps): ...`); follow the convention above for new work.
