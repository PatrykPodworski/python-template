# python-template

A template for Python CLIs that read data from a service and print it as JSON. It
comes with the checks CI runs on every pull request: ruff, mypy (strict), pytest with
90% branch coverage, vulture, deptry, mutmut (score ≥ 80%) and gitleaks.

The sample CLI is `example`. Its one command, `example status`, reads from a stub
client in `src/example_cli/client.py`, so it runs without an account or network.

## Start a project

1. Create the repo: `gh repo create <name> --public --template PatrykPodworski/python-template --clone`.
2. Rename `example` to your tool. `grep -rni example --exclude=uv.lock .` lists every
   place:
   - `pyproject.toml`: `name`, `description` and the `[project.scripts]` entry.
   - `src/example_cli/`: rename the folder and the `example_cli` imports.
   - `src/<package>/cli.py`: `NAME`, `ISSUES` and the parser description.
   - `skills/example/`: rename the folder and fill in `SKILL.md`.
   - `CLAUDE.md`: the title and the list of private data the service returns.
   - Test strings that start with `example:`.
3. Run `uv lock`, then the checks below.
4. Replace `connect` in `client.py` with the service's login, and `StubClient` with
   its API client. Replace `status.py` with your first real command.
5. Set what a template does not copy: Dependabot alerts and branch protection on
   `main`.

## Install

```sh
uv tool install git+https://github.com/PatrykPodworski/example-cli
```

This puts `example` on your `PATH`. From a clone, run it with `uv run example`.

## Commands

```sh
example status        # summary
example status --raw  # the full service response
```

Results are JSON on stdout. An error is one stderr line,
`example: <What happened.> <What to do.>`, with exit code 1, or 2 for bad arguments.

## Checks

```sh
uv run ruff check
uv run ruff format --check
uv run typos
uv run mypy
uv run pytest
uv run vulture
uv run deptry src
uv run mutmut run
```
