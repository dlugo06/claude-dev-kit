# {Project Name} — Quick Reference

**Project**: TODO: One-line project description
**Status**: TODO: Current phase/milestone
**Tech Stack**: TODO: Python 3.10+, PostgreSQL, etc.

## Quick Start

See [Quick Start Guide](.claude/guides/quick-start.md)

## Critical Rules

- Be concise. Do not explain code you've written unless asked.
- Do not add implementation summaries at the end of tasks.
- Prefer code over explanation.
- **TDD mandatory**: RED → GREEN → REFACTOR → commit. Never commit without tests.
- **Never commit to master**. Always use feature branches: `feat/`, `fix/`, `refactor/`, `test/`, `docs/`.
- **Never commit `.env`** (contains API keys and secrets).
- Internal docs (summaries, plans, research) go in `.dev/` (gitignored). Public docs in `docs/`.

## Architecture

See [Architecture Guide](.claude/guides/architecture.md)

## Development Workflows

See [Workflows Guide](.claude/guides/workflows.md)

## PR Workflow & Specialist Agents

See [PR Workflow Guide](.claude/guides/pr-workflow.md)

## Gotchas

See [Gotchas Guide](.claude/guides/gotchas.md)

## Documentation

- **PRDs**: `docs/prd/phase*.json` (tasks), `docs/prd/discussions.json` (deferred decisions)
- **Tech docs**: `docs/`
- **Agent reports** (gitignored): `.dev/`
