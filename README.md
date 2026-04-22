# Agent Boilerplate

Boilerplate for setting up your coding agent in a new repository.

## Setup

1. Copy `CLAUDE.md` and `settings.json` to the root of your new repo.
2. Update `CLAUDE.md` with your project's stack, commands, and conventions.
3. Update `settings.json` permissions to match the tools your project uses.

## Files

- `CLAUDE.md` — Project conventions loaded by the agent at startup. Defines commands, stack, and rules.
- `settings.json` — Agent permissions (allow/deny lists) and hooks. See [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code/settings).

## Folders

- `agents/` — Custom agent definitions with specialized roles or personas.
- `rules/` — Reusable rule files that can be imported into `CLAUDE.md`.
- `skills/` — Skill packs with domain-specific instructions (e.g. TDD, architecture review).