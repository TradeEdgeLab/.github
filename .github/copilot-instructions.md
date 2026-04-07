# TradeEdgeLab — Copilot Instructions

## Scope

- Default scope is TradeEdgeLab repositories only (MarketEnvConfirmSystem, MarketKnowledgeLayer, and future additions).
- EODHD org repositories are out of scope unless explicitly referenced.
- Never run commands or access anything outside the current repository's project directory.
- uv environments are local (`cache-dir` inside each repo). No global installs.
- Any action that would change global system state must be refused immediately.

## Python

- Target Python 3.12. Use modern syntax: `X | Y` unions (PEP 604), `from __future__ import annotations`.
- All code must pass `ruff check` and `ruff format --check` before submission.
- All code must pass `mypy` with the project's strict configuration.
- Line length: 99. Quote style: double. Docstring format: google.
- Use `uv run` to execute project tools (e.g., `uv run ruff`, `uv run mypy`, `uv run pytest`).

## Commits

- Follow Conventional Commits: `type(scope): description`.
- Every commit must have a body explaining what and why.
- Every commit must reference a GitHub issue (e.g., `Refs: #123` or `Closes #123`).
- Scope is freeform — use whatever describes the area of change.
- Valid types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`, `perf`, `build`.

## Code Changes

- Make minimal, surgical edits. Do not touch unrelated files.
- Run `make check-all` (or equivalent) to validate before finishing.
- Do not add unnecessary abstractions, error handling, or documentation beyond what was requested.

## GitHub API Quirks for Agent Workflows

- The `gh` CLI does not natively support creating official GitHub Epic/Sub-issue relationships.
- Editing an issue body to include markdown task lists (e.g., `- [ ] #123`) does **not** create native parent/child "Sub-issue" relations.
- You **must** bypass `gh issue edit` and use the GitHub GraphQL API directly with the `addSubIssue` mutation to establish these relationships.
- Use the following pattern to manually link an issue to an Epic:
  ```bash
  # Step 1: Retrieve GraphQL Node IDs
  PARENT_NODE_ID=$(gh issue view <EPIC_NUMBER> --json id -q .id)
  CHILD_NODE_ID=$(gh issue view <CHILD_NUMBER> --json id -q .id)

  # Step 2: Use the addSubIssue mutation
  gh api graphql -f query='
    mutation AddSubIssue($epicId: ID!, $childId: ID!) {
      addSubIssue(input: {issueId: $epicId, subIssueId: $childId}) {
        issue { id }
      }
    }
  ' -f epicId="$PARENT_NODE_ID" -f childId="$CHILD_NODE_ID"
  ```
