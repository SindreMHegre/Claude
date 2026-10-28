---
name: merge-request-summary
description: Create a concise merge request / pull request summary from the committed changes on the current branch. Use when the user asks for an MR summary, merge request summary, PR description, or branch change summary.
compatibility: opencode
metadata:
  focus: git
  audience: developers
---

## What I do

I compare the committed changes on the current branch against the base branch
and write a short, scannable summary suitable for a merge request or pull
request description.

## When to use me

- The user asks for an MR/PR summary or description
- The user wants a quick overview of what a branch changes before opening a
  merge request

## Process

1. **Find the base branch**: Use the base branch the user names. Otherwise use
   the repository's default branch (usually `main`), for example from
   `git symbolic-ref refs/remotes/origin/HEAD`.
2. **Collect committed changes**: Compare against the merge base, for example
   `git log <base>..HEAD` and `git diff <base>...HEAD`. Exclude uncommitted
   working-tree changes.
3. **Understand intent**: Inspect surrounding code when needed to understand
   the purpose of the changes.
4. **Offer extra context**: Before writing, ask the user whether they want to
   paste the related issue or task description. If they decline, use only the
   committed changes. Use the description to understand the intended outcome,
   but treat the committed changes as the source of truth and do not copy the
   description into the summary uncritically.
5. **Write the summary** following the rules below.

## Rules

### Language

Write in English unless the user asks for another language. When writing in
another language, keep names and technical terms in English where that is how
developers normally use them. Never translate function names, class names,
variable names, file names, commands, identifiers, or product names. Keep
established terms such as `config`, `API`, `endpoint`, `dependency`,
`database`, `frontend`, and `backend` in English.

### Structure

Start with a short statement that prioritises the main purpose of the changes
and what they accomplished. Then add a short bullet list of supporting
implementation details. Include relevant tests and `config` impact among the
bullets when applicable.

### Style

Keep the summary quick to scan:

- Prefer 2-5 concise bullets.
- Describe meaningful outcomes and implementation choices, not every changed
  file or commit.
- Avoid headings unless they improve readability.
- Avoid speculation, review findings, and unnecessary background.
- Do not mention absent test or `config` changes merely to fill the format.

## Output format

```markdown
Add a shared retry helper for outbound HTTP calls and use it in the ingestion
jobs.

- Adds `retry_with_backoff()` in `utils/http.py` with configurable attempts and
  jitter.
- Replaces ad-hoc retry loops in the ingestion jobs with the shared helper.
- Adds pytest coverage for retry exhaustion and successful recovery.
- Introduces `HTTP_MAX_RETRIES` in `config` (defaults to 3).
```
