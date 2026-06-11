---
name: steal-skill
description: Adapt a skill found online or from another repo into a new skill that fits this project's conventions and stack. Use when the user shares a skill (as text, URL, or file) and wants to add it to the repository.
compatibility: opencode
metadata:
  focus: skills
  audience: developers
---

## What I do

I take a skill from an external source, strip out anything repo-specific, and rewrite it to fit this project's conventions — then save it as a new skill file.

## Process

1. **Ingest**: Read the source skill (from pasted text, a URL, or a file path the user provides).
2. **Analyse**: Identify what the skill does at its core, and what parts are tied to a specific stack, language, or organisation.
3. **Ask (if needed)**: If the skill's purpose is ambiguous, ask one clarifying question before proceeding.
4. **Generalise**: Remove hardcoded references (specific frameworks, company names, internal tooling) unless they match this project's stack (Python 3.12, pytest, ruff, Docker).
5. **Adapt**: Rewrite examples and language conventions to match this project (Python docstrings over Javadoc, `pathlib` over `os.path`, etc.).
6. **Save**: Create the skill at `.claude/skills/<kebab-case-name>/SKILL.md` with correct frontmatter.

## Output skill format

```markdown
---
name: <kebab-case-name>
description: <one sentence — what it does and when to invoke it>
compatibility: opencode
metadata:
  focus: <topic>
  audience: developers
---

## What I do
...

## When to use me
...

## Process / Rules
...

## Output format (if applicable)
...
```

## Frontmatter rules

- `name`: lowercase kebab-case, matches directory name
- `description`: start with an action verb; include trigger phrases users might say
- `compatibility`: always `opencode`
- Drop metadata keys that don't apply

## What to strip

- Language-specific sections for languages not used in this project (e.g. Wicket, Kafka, C#)
- Internal tool names, team names, or organisation-specific jargon
- Severity scales or rating systems that don't generalise — simplify or replace with plain priority labels

## What to keep

- The core workflow and mental model of the skill
- Any output format that is useful and generic
- Edge-case guidance that applies broadly

## After saving

Report:
- The path where the skill was saved
- A one-line summary of what was changed vs the original
- Any assumptions made during generalisation
