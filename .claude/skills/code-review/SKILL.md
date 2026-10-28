---
name: code-review
description: Review the committed changes on the current branch for correctness, regressions, maintainability, and missing tests. Use when the user asks for a code review, PR review, MR review, or branch review.
compatibility: opencode
metadata:
  focus: code-review
  audience: developers
---

## What I do

I review the committed changes on the current branch against its base branch
and report concrete, actionable findings, each paired with a recommended
change.

## When to use me

- The user asks for a code review, PR review, MR review, or branch review
- Before opening or merging a merge request / pull request

## Process

1. **Find the base branch**: Use the base branch the user names. Otherwise use
   the repository's default branch (usually `main`), for example from
   `git symbolic-ref refs/remotes/origin/HEAD`.
2. **Collect committed changes only**: Exclude uncommitted working-tree
   changes.

   ```bash
   git diff <base>...HEAD
   git log <base>..HEAD --oneline
   ```

3. **Read context**: Inspect relevant surrounding code, tests, and repository
   instructions (`CLAUDE.md`, `AGENTS.md`, `README.md`, `.claude/rules/`, and
   tool config such as `pyproject.toml`).
4. **Review** using the rules below.
5. **Report** using the output format below.

## Rules

### Priorities

Prioritise findings that can cause wrong results, regressions, data loss,
security issues, or operational failures. Do not report formatting or lint
issues already enforced by tooling (for example `ruff check` / `ruff format`).

### What to check

- Logic errors and incorrect assumptions about input data, including units,
  time zones, indexing, and missing or empty values.
- Changed public behaviour: function signatures, configuration, file paths,
  external API or database calls, and error handling.
- Secrets or sensitive data committed or exposed.
- Tests (pytest) covering normal cases, edge cases, and the changed
  behaviour.
- Project conventions documented in the repository instructions, such as
  required type hints, `pathlib` over `os.path`, no `import *`, specific
  exception handling, and test file placement.

### Code smells

Also report these code smells as suggestions when fixing them would make the
code clearer, simpler, or easier to change. Mark them as judgement calls
rather than defects:

- **Mysterious Name**: a name does not make its purpose or value clear.
- **Duplicated Code**: the changed code repeats meaningful logic.
- **Feature Envy**: a function depends more on another object's data than on
  its own.
- **Primitive Obsession**: a primitive represents a domain concept that needs
  validation or clearer meaning.
- **Middle Man**: a class or function mainly delegates without adding value.

### Honesty

Do not invent issues. If there are no actionable findings, say so and note any
testing gaps or assumptions.

## Output format

Report findings under `## Findings and Recommended Changes`, ordered by
priority (`High`, `Medium`, `Low`). Group each finding with its recommended
change in a numbered list:

```markdown
## Findings and Recommended Changes

1. **[High] `src/ingest/loader.py:42` - Timestamps parsed without a time zone**
   Finding: `datetime.fromisoformat()` returns naive datetimes, which are
   later compared against UTC-aware values and raise `TypeError`.
   Recommended change: Parse with an explicit `tzinfo=UTC` and add a pytest
   case for a timestamp without an offset.

2. **[Low, judgement call] `src/ingest/loader.py:88` - Mysterious Name**
   Finding: `tmp2` holds the deduplicated records, which is not clear from
   the name.
   Recommended change: Rename it to `unique_records`.
```

End with a brief summary of what was reviewed.
