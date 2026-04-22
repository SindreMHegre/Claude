# Project conventions
You should always have a claude.md file or similar main readme file for the agent you are using

## Commands
- Build: `docker build .`
- Test: `pytest`
- Lint: `ruff check .`
- Format: `ruff format .`

## Stack
- python 3.12
- docker
- pytest
- ruff

## Rules
- Tests live next to source: `foo.py` → `foo_test.py`
- Use type hints for all function signatures
- Prefer `pathlib` over `os.path`
- Never use `import *`
- All API routes return `{ data, error }` shape
