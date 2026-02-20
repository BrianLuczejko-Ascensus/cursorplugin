---
name: testing-standards
description: Testing conventions, coverage requirements, and best practices
---

# Testing Standards

These rules define the testing conventions enforced during reviews and analysis.

## Coverage Requirements

- **Minimum overall coverage:** 80% line coverage
- **Critical paths (auth, payments, data):** 90% branch coverage
- **New code:** Must include tests; PRs that lower coverage will be flagged

## Test Structure

Follow the Arrange-Act-Assert pattern:

```
1. Arrange — set up test data and dependencies
2. Act — call the function under test
3. Assert — verify the result
```

## Naming

- Test files: `<module>.test.ts` or `test_<module>.py`
- Test names should describe the behavior, not the implementation:
  - Good: `should return null when user is not found`
  - Bad: `test getUserById`

## What to Test

Every public function should have tests for:
- **Happy path** — valid inputs produce expected outputs
- **Edge cases** — empty strings, zero, null/undefined, empty arrays
- **Error cases** — invalid inputs, network failures, timeouts
- **Boundary conditions** — max values, off-by-one, unicode

## What NOT to Test

- Private implementation details (test through the public API)
- Third-party library internals
- Trivial getters/setters with no logic
- Framework boilerplate

## Mocking Guidelines

- Mock external dependencies (HTTP, databases, file system)
- Do NOT mock the unit under test
- Prefer dependency injection over monkey-patching
- Reset mocks between tests to avoid state leakage
- Use realistic fake data, not placeholder strings like "test" or "foo"

## Test Isolation

- Tests must not depend on execution order
- Tests must not share mutable state
- Each test should set up and tear down its own fixtures
- Use transactions or in-memory databases for integration tests
