---
name: devpulse-refactor
description: Identify and execute safe refactoring operations on codebases. Use this when asked to refactor code, reduce complexity, extract functions, eliminate duplication, or restructure modules.
---

# DevPulse Refactor

You are a specialized skill for identifying refactoring opportunities and executing safe, incremental transformations.

## When to Use This Skill

Invoke this skill when:
- User asks to "refactor this code" or "clean this up"
- User wants to "reduce complexity" or "simplify"
- User requests "extract a function" or "break this apart"
- User mentions "code smells" or "technical debt"
- DevPulse code review identified maintainability issues

## Refactoring Catalog

### Extract Function
**When:** A block of code does one identifiable thing inside a larger function.

Before:
```typescript
function processOrder(order: Order) {
  // ... 20 lines of validation logic ...
  // ... 15 lines of payment logic ...
  // ... 10 lines of notification logic ...
}
```

After:
```typescript
function processOrder(order: Order) {
  validateOrder(order);
  processPayment(order);
  sendConfirmation(order);
}
```

### Replace Nested Conditionals with Guard Clauses
**When:** Deep nesting makes control flow hard to follow.

Before:
```typescript
function getDiscount(user: User) {
  if (user) {
    if (user.membership) {
      if (user.membership.isActive) {
        return user.membership.discountRate;
      }
    }
  }
  return 0;
}
```

After:
```typescript
function getDiscount(user: User) {
  if (!user?.membership?.isActive) return 0;
  return user.membership.discountRate;
}
```

### Consolidate Duplicate Code
**When:** The same logic appears in multiple places.

Use the DevPulse MCP server duplication report to identify candidates:
```bash
devpulse analyze --duplication src/
```

### Replace Magic Values with Constants
**When:** Literal numbers or strings appear without explanation.

Before:
```typescript
if (retries > 3) { ... }
if (status === "PROC_COMPLETE") { ... }
setTimeout(fn, 86400000);
```

After:
```typescript
const MAX_RETRIES = 3;
const STATUS_COMPLETE = "PROC_COMPLETE";
const ONE_DAY_MS = 24 * 60 * 60 * 1000;
```

### Introduce Parameter Object
**When:** A function takes more than 3-4 related parameters.

Before:
```typescript
function createUser(name: string, email: string, age: number, role: string, team: string) { ... }
```

After:
```typescript
interface CreateUserParams {
  name: string;
  email: string;
  age: number;
  role: string;
  team: string;
}

function createUser(params: CreateUserParams) { ... }
```

## Safety Workflow

1. **Analyze** — Read the code and understand all call sites
2. **Plan** — Describe the refactoring to the user before executing
3. **Execute** — Make small, incremental changes
4. **Verify** — Run tests after each change to confirm nothing broke

## Guidelines

- Never change behavior during a refactoring — only restructure
- Prefer small, reviewable changes over large sweeping rewrites
- Always check for existing tests before refactoring; if none exist, suggest adding them first
- Use the project's existing naming conventions and patterns
- If a refactoring touches more than 5 files, break it into separate steps
