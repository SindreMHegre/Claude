---
name: documentation-checker
description: Ensure public APIs and complex logic have proper documentation
compatibility: opencode
metadata:
  focus: documentation
  language: python,typescript,java
  audience: developers
---

## What I do

I check that public APIs and complex logic have appropriate documentation.

## When to use me

Use this skill when reviewing:
- New public functions or classes
- Complex algorithms
- REST API endpoints
- Shared libraries or modules

## Documentation requirements

### Python

**Public Functions and Methods** (Severity 7)
- Docstring with description, `Args`, `Returns`, `Raises`
- Follow Google or NumPy docstring style

**Public Classes** (Severity 6)
- Class-level docstring describing purpose and usage
- Document `__init__` parameters

**Complex Algorithms** (Severity 6)
- Explain the "why", not just the "what"
- Inline comments for non-obvious logic

**REST API Endpoints** (Severity 9)
- Document parameters, response shape, and auth requirements

**Module-level** (Severity 4)
- Brief module docstring describing purpose

### TypeScript / JavaScript

**Exported Functions** (Severity 7)
- JSDoc with description, `@param`, `@returns`, `@throws`

**Complex Types** (Severity 5)
- Document type parameters and union types

### Java

**Public Methods** (Severity 8)
- Javadoc with description, `@param`, `@return`, `@throws`

**REST Endpoints** (Severity 9)
- Document parameters, responses, permissions

## Output format

For each issue:

```
### 📚 [Severity X] <Issue Title>
**Location**: file.py:45

**Issue**: <description>

**Should Include**:
<example documentation>
```

## Severity guide

- **9-10**: Critical public APIs (REST endpoints)
- **7-8**: Public functions, services, components
- **5-6**: Complex algorithms, public classes
- **3-4**: Internal helpers, module docstrings
- **1-2**: Obvious code

## Edge cases

- Don't require docs for private helpers (`_foo`), tests, or trivial properties
- Quality over quantity — explain "why" not "what"
- If no issues found, report: "Documentation is adequate."
