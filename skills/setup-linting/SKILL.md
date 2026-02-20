---
name: setup-linting
description: Configure linters and formatters for any project. Use this when asked to setup ESLint, Prettier, Ruff, RuboCop, golangci-lint, or similar tools. Supports JavaScript, TypeScript, Python, Ruby, Go, and Rust.
---

# Setup Linting & Formatting

This skill helps configure linters and code formatters to enforce consistent code style and catch common errors.

## When to Use This Skill

Invoke this skill when:
- User asks to "setup linting" or "add ESLint"
- User wants to "configure Prettier" or "add a formatter"
- User requests "code style enforcement"
- User mentions ESLint, Prettier, Ruff, RuboCop, or Biome

## Platform Detection

Detect the project platform and existing config:

### JavaScript/TypeScript
- Check for existing `.eslintrc.*`, `eslint.config.*`, `.prettierrc.*`
- Check `package.json` for framework (React, Vue, Next.js, Node)
- Recommend: **ESLint v9 flat config** + **Prettier**
- Alternative: **Biome** (all-in-one)

### Python
- Check for existing `pyproject.toml`, `ruff.toml`, `.flake8`
- Recommend: **Ruff** (linting + formatting)

### Ruby
- Check for `.rubocop.yml`
- Recommend: **RuboCop**

### Go
- Built-in `gofmt` + recommend **golangci-lint**

---

## JavaScript/TypeScript Setup

### ESLint v9 + Prettier

```bash
npm install -D eslint @eslint/js typescript-eslint prettier eslint-config-prettier
```

**`eslint.config.js`:**
```javascript
import js from "@eslint/js";
import tseslint from "typescript-eslint";
import prettierConfig from "eslint-config-prettier";

export default [
  js.configs.recommended,
  ...tseslint.configs.recommended,
  prettierConfig,
  {
    rules: {
      "no-console": "warn",
      "@typescript-eslint/no-unused-vars": ["error", { argsIgnorePattern: "^_" }],
      "@typescript-eslint/no-explicit-any": "warn",
    },
  },
  {
    ignores: ["dist/", "node_modules/", "coverage/"],
  },
];
```

**`.prettierrc`:**
```json
{
  "semi": true,
  "singleQuote": false,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 100
}
```

**Add to `package.json` scripts:**
```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

---

## Python Setup

### Ruff (Recommended)

```bash
pip install ruff
```

**Add to `pyproject.toml`:**
```toml
[tool.ruff]
target-version = "py312"
line-length = 88

[tool.ruff.lint]
select = ["E", "F", "I", "N", "W", "UP", "B", "SIM", "RUF"]
ignore = ["E501"]

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
```

**Usage:**
```bash
ruff check .          # Lint
ruff check . --fix    # Lint + autofix
ruff format .         # Format
```

---

## Verification

After setup, run the linter and fix any initial violations:

```bash
# JavaScript/TypeScript
npm run lint
npm run format

# Python
ruff check . --fix && ruff format .

# Ruby
bundle exec rubocop -a
```

## Pre-commit Hook (Optional)

Recommend setting up a pre-commit hook to enforce linting:

```bash
npx husky init
echo "npm run lint && npm run format:check" > .husky/pre-commit
```

Or for Python with `pre-commit`:
```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.8.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
```
