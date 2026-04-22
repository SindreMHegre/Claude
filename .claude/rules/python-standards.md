---
paths:
  - "**/*.py"
---

# Python Standards and Style

## Type Hints
- All function signatures must have type hints, including return types
- Use `from __future__ import annotations` for forward references
- Use `X | Y` union syntax over `Union[X, Y]` (Python 3.10+)
- Prefer `list[str]` over `List[str]`, `dict[str, int]` over `Dict[str, int]`
- Use `TypeAlias` for complex type aliases

## Imports
- Never use `import *`
- Group imports: stdlib → third-party → local, separated by blank lines
- Prefer absolute imports over relative imports

## Naming
- `snake_case` for variables, functions, and modules
- `PascalCase` for classes
- `UPPER_SNAKE_CASE` for module-level constants
- Prefix private members with `_`, not `__` unless name mangling is intentional

## Functions and Classes
- Keep functions small and single-purpose
- Prefer `dataclasses` or `pydantic` models over plain dicts for structured data
- Use `@staticmethod` or module-level functions instead of class methods that don't use `self`

## Error Handling
- Catch specific exceptions, never bare `except:`
- Raise exceptions with context: `raise ValueError("msg") from err`
- Use custom exception classes for domain errors

## File I/O and Paths
- Always use `pathlib.Path` over `os.path`
- Use context managers (`with`) for file operations

## Testing
- Tests live next to source: `foo.py` → `foo_test.py`
- Use `pytest` with `assert` statements, not `unittest.TestCase`
- Name test functions descriptively: `test_<function>_<scenario>_<expected>`
- Mock external I/O; do not make real network or filesystem calls in unit tests

## Tooling
- Lint: `ruff check .`
- Format: `ruff format .`
- Type check: `mypy .` (if configured)
