---
name: setup-testing
description: Setup test frameworks and generate test scaffolding for any project. Use this when asked to add testing, configure Jest/Vitest/pytest/RSpec, or generate test files. Supports JavaScript, TypeScript, Python, Ruby, Go, and Rust.
---

# Setup Testing

This skill helps configure test frameworks, generate test scaffolding, and establish testing conventions for any project.

## When to Use This Skill

Invoke this skill when:
- User asks to "setup testing" or "add a test framework"
- User wants to "generate tests" or "create test files"
- User requests "test scaffolding" for a module
- User mentions Jest, Vitest, pytest, RSpec, or similar frameworks
- User asks about test coverage configuration

## Platform Detection

Before configuring, detect the project platform:

### JavaScript/TypeScript
Check `package.json` for:
- `vitest` — Vitest (Vite-based projects)
- `jest` — Jest
- `@testing-library/react` — React Testing Library
- `cypress` — Cypress E2E
- `playwright` — Playwright E2E

### Python
Check for `pytest` in requirements or `pyproject.toml`

### Ruby
Check for `rspec` in Gemfile

### Go
Built-in `testing` package (no setup needed)

### Rust
Built-in `#[test]` attribute (no setup needed)

---

## JavaScript/TypeScript Configuration

### Vitest (Recommended for Vite projects)

```bash
npm install -D vitest @vitest/coverage-v8
```

**`vitest.config.ts`:**
```typescript
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    globals: true,
    environment: "jsdom",
    coverage: {
      provider: "v8",
      reporter: ["text", "html", "lcov"],
      thresholds: {
        statements: 80,
        branches: 80,
        functions: 80,
        lines: 80,
      },
    },
    include: ["src/**/*.test.{ts,tsx}"],
  },
});
```

### Jest

```bash
npm install -D jest @types/jest ts-jest
```

**`jest.config.ts`:**
```typescript
import type { Config } from "jest";

const config: Config = {
  preset: "ts-jest",
  testEnvironment: "jsdom",
  collectCoverageFrom: ["src/**/*.{ts,tsx}", "!src/**/*.d.ts"],
  coverageThreshold: {
    global: {
      statements: 80,
      branches: 80,
      functions: 80,
      lines: 80,
    },
  },
};

export default config;
```

### Test File Template (JavaScript/TypeScript)

```typescript
import { describe, it, expect, beforeEach, vi } from "vitest";
import { MyService } from "./my-service";

describe("MyService", () => {
  let service: MyService;

  beforeEach(() => {
    service = new MyService();
  });

  describe("methodName", () => {
    it("should handle the happy path", () => {
      const result = service.methodName("valid-input");
      expect(result).toEqual(expectedOutput);
    });

    it("should throw on invalid input", () => {
      expect(() => service.methodName("")).toThrow("Input required");
    });

    it("should handle edge cases", () => {
      const result = service.methodName(null);
      expect(result).toBeNull();
    });
  });
});
```

---

## Python Configuration

### pytest

```bash
pip install pytest pytest-cov pytest-asyncio
```

**`pyproject.toml`:**
```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
python_classes = "Test*"
python_functions = "test_*"
addopts = "--cov=src --cov-report=term-missing --cov-fail-under=80"

[tool.coverage.run]
source = ["src"]
omit = ["tests/*", "*/migrations/*"]
```

### Test File Template (Python)

```python
import pytest
from myapp.services import MyService


class TestMyService:
    @pytest.fixture
    def service(self):
        return MyService()

    def test_happy_path(self, service):
        result = service.method_name("valid-input")
        assert result == expected_output

    def test_invalid_input_raises(self, service):
        with pytest.raises(ValueError, match="Input required"):
            service.method_name("")

    def test_edge_case_none(self, service):
        result = service.method_name(None)
        assert result is None

    @pytest.mark.asyncio
    async def test_async_operation(self, service):
        result = await service.async_method()
        assert result.status == "success"
```

---

## Scaffolding Strategy

When generating tests for an existing module:

1. **Read the source file** to understand all public methods
2. **Identify test cases** for each method:
   - Happy path (valid inputs, expected behavior)
   - Edge cases (empty strings, zero, None/null)
   - Error cases (invalid input, network failures)
   - Boundary conditions (max values, empty collections)
3. **Generate the test file** following the project's conventions
4. **Add mock/stub setup** for external dependencies

## Verification

After setup:
```bash
# JavaScript/TypeScript
npm test
npm run test -- --coverage

# Python
pytest
pytest --cov

# Ruby
bundle exec rspec
```
