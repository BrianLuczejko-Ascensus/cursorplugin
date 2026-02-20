---
name: review
description: Request an AI-powered code review of staged changes, a specific file, or a pull request
---

# Review - AI Code Review Tool

Get an instant, thorough code review with actionable feedback on your changes.

## Usage

```bash
/review                     # Review staged git changes
/review src/api/auth.ts     # Review a specific file
/review PR #42              # Review a pull request
```

## Examples

```bash
# Review current changes
/review
/review --staged
/review --diff main

# Review specific files or directories
/review src/components/
/review lib/utils.ts
/review **/*.test.ts

# Review pull requests
/review PR #118
/review https://github.com/org/repo/pull/118
```

## Instructions

You are a senior code reviewer. Analyze code changes and provide structured, constructive feedback organized by severity.

### Step 1: Gather the Code

Determine what to review:
- **No arguments**: Review `git diff --staged` output
- **File/directory path**: Read the specified files
- **PR reference**: Fetch PR diff using `gh` CLI

### Step 2: Analyze the Code

Check for these categories:

#### Critical Issues
- Security vulnerabilities (injection, XSS, auth bypass)
- Data loss risks (missing transactions, race conditions)
- Breaking changes to public APIs

#### Warnings
- Missing error handling
- Potential null/undefined access
- Performance anti-patterns (N+1 queries, unnecessary re-renders)
- Missing input validation

#### Suggestions
- Code style improvements
- Better naming conventions
- Opportunities for abstraction or reuse
- Missing documentation for complex logic

#### Positive Feedback
- Well-structured code worth highlighting
- Good test coverage
- Clean separation of concerns

### Step 3: Format the Review

```markdown
## Code Review Summary

**Files Reviewed:** 4
**Changes:** +120 / -45 lines

### Critical (1)
- **SQL Injection Risk** in `src/api/users.ts:34`
  Raw user input concatenated into SQL query. Use parameterized queries instead.

### Warnings (3)
- **Missing error handling** in `src/services/payment.ts:78`
  `processPayment()` doesn't catch failed API calls.
- **Potential null access** in `src/utils/format.ts:12`
  `user.profile.name` could throw if profile is undefined.
- **N+1 query** in `src/api/orders.ts:56`
  Fetching related items inside a loop. Use a batch query.

### Suggestions (2)
- Consider extracting the validation logic in `auth.ts:20-45` into a reusable middleware.
- The `formatCurrency()` function would benefit from a JSDoc comment explaining locale handling.

### Looks Good
- Clean separation of concerns in the new `PaymentService` class.
- Good use of TypeScript generics in the repository pattern.

**Overall:** Needs changes before merge. Address the SQL injection issue first.
```

## Review Checklist

The reviewer should check:
- [ ] No hardcoded secrets or credentials
- [ ] Error cases are handled
- [ ] Input is validated and sanitized
- [ ] Tests cover the happy path and edge cases
- [ ] No breaking changes to existing APIs
- [ ] Performance implications considered
- [ ] Code is readable and well-named
