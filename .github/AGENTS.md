# TradeEdgeLab — Organization Agent Instructions

## Scope

- Default scope is TradeEdgeLab repositories only.
- Never run commands or access anything outside the current repository's project directory.
- uv environments are local. No global installs or state changes.

## Python Standards

- Python 3.12, modern syntax (`from __future__ import annotations`, PEP 604 unions).
- All code must pass `ruff check .`, `ruff format --check .`, and `mypy`.
- Line length: 99. Double quotes. Google docstrings.
- Use `uv run` for all tool execution.

## Commits

- Conventional Commits: `type(scope): description` with body and issue reference.
- Valid types: feat, fix, docs, style, refactor, test, chore, ci, perf, build.
- Scope is freeform.

## Pre-Commit

- Pre-commit hooks must pass before any commit.
- Run `make check-all` to validate all checks locally.

## Code Changes

- Minimal, surgical edits. Do not touch unrelated files.
- Do not invent missing facts or add unnecessary abstractions.
