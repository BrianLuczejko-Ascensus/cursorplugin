---
name: setup-ci
description: Configure CI/CD pipelines for GitHub Actions, GitLab CI, or CircleCI. Use this when asked to add continuous integration, setup automated testing in CI, or create deployment workflows.
---

# Setup CI/CD

This skill helps configure continuous integration and deployment pipelines for any project.

## When to Use This Skill

Invoke this skill when:
- User asks to "setup CI" or "add GitHub Actions"
- User wants "automated testing in CI"
- User requests a "deployment pipeline" or "CD workflow"
- User mentions GitHub Actions, GitLab CI, or CircleCI

## Platform Detection

Detect the CI platform and project type:

### CI Platform
- `.github/workflows/` exists — GitHub Actions
- `.gitlab-ci.yml` exists — GitLab CI
- `.circleci/config.yml` exists — CircleCI
- None — ask user preference (default: GitHub Actions)

### Project Type
- `package.json` — Node.js / JavaScript / TypeScript
- `pyproject.toml` or `requirements.txt` — Python
- `Gemfile` — Ruby
- `go.mod` — Go
- `Cargo.toml` — Rust

---

## GitHub Actions Templates

### Node.js CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: "npm"

      - run: npm ci
      - run: npm run lint
      - run: npm test -- --coverage
      - run: npm run build

      - name: Upload coverage
        if: matrix.node-version == 20
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: coverage/
```

### Python CI

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12"]

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
          cache: "pip"

      - run: pip install -r requirements.txt
      - run: pip install -r requirements-dev.txt
      - run: ruff check .
      - run: pytest --cov --cov-report=xml
```

---

## Workflow Structure

Every CI pipeline should include:

1. **Lint** — Static analysis and formatting checks
2. **Test** — Unit and integration tests with coverage
3. **Build** — Verify the project compiles/bundles successfully
4. **Security** — Dependency vulnerability scanning (optional)
5. **Deploy** — Deployment to staging/production (CD, optional)

## Guidelines

- Pin action versions to major tags (e.g., `actions/checkout@v4`)
- Use caching for dependencies to speed up builds
- Run the full matrix only on PRs; use a single version for pushes to main if speed matters
- Store secrets in GitHub Actions secrets, never in workflow files
- Add status badges to README after setup
