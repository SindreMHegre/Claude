---
name: git-diff-analysis
description: Analyze git branch changes and identify modified files
compatibility: opencode
metadata:
  focus: git-workflow
  language: any
  audience: developers
---

## What I do

I generate a diff of the current branch compared to origin/master, providing:
- Current branch name
- Diff mode used (and why)
- List of changed files
- Full diff output for analysis by other skills

## Diff Modes

| Mode | What it diffs | Use case |
|------|--------------|----------|
| `branch` | Committed changes: `origin/master..HEAD` | Reviewing a feature branch before or after push |
| `local` | Working tree vs origin/master: `origin/master` | Reviewing all local work (committed + uncommitted) before pushing |

## When to use me

Use this skill as the first step in any workflow that needs to understand branch changes:
- Code reviews
- PR description generation
- Before running other analysis skills

## How I work

### Step 1: Determine diff mode

The caller may specify an explicit mode (`--local` or `--branch`). If no mode is specified, auto-detect:

```bash
# Check for uncommitted changes to tracked files (unstaged)
git --no-pager -c color.ui=false diff --name-only HEAD
# Check for uncommitted changes to tracked files (staged)
git --no-pager -c color.ui=false diff --cached --name-only HEAD
```

- If **either command returns output** → there are uncommitted tracked-file changes → default to **local** mode
- If **neither returns output** → no uncommitted changes → default to **branch** mode

**Important**: Untracked files (new files not yet `git add`ed) do NOT trigger local mode.

Report which mode was selected and whether it was explicit or auto-detected.

### Step 2: Get current branch
```bash
git --no-pager -c color.ui=false branch --show-current
```

### Step 3: Validate branch

- **branch mode**: If on `master` or `main` → **Stop** and report error. There are no branch commits to diff.
- **local mode**: Any branch is allowed, including `master`/`main` (reviewing uncommitted work on any branch is valid).

### Step 3b: Check for stale origin/master (branch mode only)

In `branch` mode, diff against a stale `origin/master` can silently produce misleading results (missing master's advances, inflating the diff). Before diffing, verify the local ref matches the remote:

```bash
# SHA of local origin/master ref
git --no-pager -c color.ui=false rev-parse origin/master

# SHA of the actual remote master HEAD (network call)
git --no-pager -c color.ui=false ls-remote origin master
```

Compare the two SHAs:
- If they **match** → origin/master is up to date, continue.
- If they **differ** → origin/master is stale → **Stop** and report the error below.

**Do not run this check in `local` mode** — `local` mode diffs the working tree vs the local `origin/master` ref, which is intentional (you're reviewing what you have locally).

### Step 4: Get changed files

**branch mode:**
```bash
git --no-pager -c color.ui=false diff origin/master..HEAD --name-only
```

**local mode:**
```bash
git --no-pager -c color.ui=false diff origin/master --name-only
```

### Step 5: Get full diff

**branch mode:**
```bash
git --no-pager -c color.ui=false diff origin/master..HEAD
```

**local mode:**
```bash
git --no-pager -c color.ui=false diff origin/master
```

## Output format

Return the raw git output in a structured format:

```
## Mode
<local | branch> (<auto-detected | explicit>)

## Branch
<branch-name>

## Changed Files
<output from --name-only>

## Diff
<full diff output>
```

## Error conditions

### On master/main branch (branch mode only)
```
Error: Cannot generate diff - currently on master/main branch.
Switch to a feature branch first, or use --local to review uncommitted changes.
```

### Stale origin/master (branch mode only)
```
Error: Local origin/master is out of date.
  Local SHA:  <local-sha>
  Remote SHA: <remote-sha>

Run: git fetch origin master
Then re-run the review.
```

### No changes
```
Branch: <branch-name>
Mode: <mode>
No changes detected.
```

## Important notes

- Always use `--no-pager` and `-c color.ui=false` to prevent ANSI codes
- Do not analyze or categorize the diff - that is for other skills
- Do not add recommendations or warnings - just provide the diff
- For very large diffs, the output may be truncated by the tool
- In `local` mode, `git diff origin/master` (without `..HEAD`) compares the working tree directly to origin/master, capturing both committed and uncommitted changes in one command
