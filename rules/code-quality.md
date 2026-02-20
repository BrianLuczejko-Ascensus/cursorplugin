---
name: code-quality
description: Code quality standards and best practices enforced by DevPulse
---

# Code Quality Rules

These rules define the code quality standards that DevPulse enforces during reviews and analysis.

## Function Complexity

- **Max cyclomatic complexity:** 10 per function
- **Max function length:** 40 lines (excluding blank lines and comments)
- **Max parameters:** 4 per function (use an options object for more)
- **Max nesting depth:** 3 levels

## Naming Conventions

- Use descriptive, intention-revealing names
- Boolean variables should start with `is`, `has`, `can`, `should`
- Functions should start with a verb: `get`, `set`, `create`, `delete`, `validate`, `transform`
- Avoid abbreviations unless universally understood (`id`, `url`, `api` are OK; `usr`, `mgr`, `proc` are not)

## Error Handling

- Never use empty catch blocks
- Always provide meaningful error messages
- Use typed/custom errors for domain-specific failures
- Log errors with enough context to debug (include relevant IDs, inputs)
- Prefer fail-fast: validate inputs at function boundaries

## Security

- Never hardcode secrets, API keys, or credentials
- Always parameterize database queries (no string concatenation)
- Sanitize user input before rendering in HTML
- Use HTTPS for all external API calls
- Apply the principle of least privilege for file and network access

## Performance

- Avoid synchronous I/O in async code paths
- Paginate unbounded list queries
- Cache expensive computations when inputs are stable
- Prefer batch operations over loops with individual calls (avoid N+1 patterns)

## Code Organization

- One module, one responsibility
- Export only what consumers need; keep internals private
- Group related functions and types in the same module
- Keep import lists organized: stdlib first, third-party second, local third
