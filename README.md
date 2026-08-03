# Project Name

One-line description of what this project does and who it's for.

## Getting Started

### Prerequisites

- List required tools, runtimes, and versions here.

### Installation

```sh
# Clone the repo and install dependencies
git clone <repo-url>
cd <repo-directory>
```

This repo includes a [Dev Container](.devcontainer/devcontainer.json) — open
it in VS Code and choose "Reopen in Container" for a consistent, pre-configured
environment.

## Usage

Describe how to run/use the project here.

## Development

This repo runs [MegaLinter](https://megalinter.io/) in CI on every push and pull
request to `main`. Config lives in [.mega-linter.yml](.mega-linter.yml).

- [.editorconfig](.editorconfig) and [.vscode/settings.json](.vscode/settings.json)
  keep editor formatting consistent (indentation, line endings, trailing whitespace).
- [.gitattributes](.gitattributes) normalizes line endings and marks binary files.
- [pre-commit](https://pre-commit.com/) hooks in
  [.pre-commit-config.yaml](.pre-commit-config.yaml) catch common issues
  locally before you push. Install with `pre-commit install`.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to file issues and submit
pull requests. Open a pull request using the provided
[PR template](.github/PULL_REQUEST_TEMPLATE.md). CODEOWNERS in
[.github/CODEOWNERS](.github/CODEOWNERS) will be requested for review.
This project follows a [Code of Conduct](CODE_OF_CONDUCT.md). See
[SECURITY.md](SECURITY.md) for how to report vulnerabilities.

## License

Licensed under the [Apache License 2.0](LICENSE).
