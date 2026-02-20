# DevPulse Cursor Plugin

Code quality analysis, automated reviews, and developer productivity tools for Cursor. Powered by MCP, slash commands, skills, and rules.

## Installation

### From Cursor Marketplace

Search for "devpulse" in the Cursor plugin marketplace, then install.

Restart Cursor to activate the plugin, then verify:

```
/help           # Should show /pulse and /review commands
/mcp            # Should show devpulse MCP server
```

### From Local Source

```
git clone https://github.com/devpulse/devpulse-for-cursor.git
cd devpulse-for-cursor
```

Add the local directory as a plugin source in Cursor settings.

## Slash Commands

### /pulse

Ask questions about your codebase health in natural language.

```
/pulse What are the most complex functions in this project?
/pulse Show me files with the highest cyclomatic complexity
/pulse Which modules have the most code duplication?
/pulse Are any of my dependencies outdated?
/pulse What's the test coverage for src/api/?
```

### /review

Request an AI-powered code review of staged changes, files, or pull requests.

```
/review                     # Review staged git changes
/review src/api/auth.ts     # Review a specific file
/review PR #42              # Review a pull request
```

## Skills

### devpulse-code-review

Deep code quality analysis — finds anti-patterns, security issues, and maintainability problems.

```
Review the auth module for code quality issues
```

### devpulse-refactor

Identifies and executes safe refactoring operations — extract functions, simplify conditionals, eliminate duplication.

```
Refactor src/services/payment.ts to reduce complexity
```

### Setup Skills

| Skill | Description |
|-------|-------------|
| `devpulse-setup-testing` | Configure test frameworks (Jest, Vitest, pytest, RSpec) and generate test scaffolding |
| `devpulse-setup-ci` | Setup CI/CD pipelines for GitHub Actions, GitLab CI, or CircleCI |
| `devpulse-setup-linting` | Configure linters and formatters (ESLint, Prettier, Ruff, Biome) |

## Rules

The plugin ships with coding standard rules that the agent follows:

| Rule | Description |
|------|-------------|
| `code-quality` | Function complexity limits, naming conventions, error handling, security |
| `testing-standards` | Coverage requirements, test structure, mocking guidelines |
| `api-design` | REST API conventions, response codes, pagination, error formats |

## Configuration

The plugin automatically configures the DevPulse MCP server on install. No additional setup required beyond restarting Cursor.

## Plugin Structure

```
devpulse-for-cursor/
├── .cursor-plugin/
│   ├── plugin.json           # Plugin metadata
│   └── marketplace.json      # Marketplace listing
├── mcp.json                  # MCP server configuration
├── AGENTS.md                 # Agent instructions
├── commands/
│   ├── pulse.md              # /pulse command
│   └── review.md             # /review command
├── skills/
│   ├── devpulse-code-review/
│   ├── devpulse-setup-testing/
│   ├── devpulse-setup-ci/
│   ├── devpulse-refactor/
│   └── devpulse-setup-linting/
├── rules/
│   ├── code-quality.md
│   ├── testing-standards.md
│   └── api-design.md
├── assets/
│   └── logo.svg              # Plugin logo
└── LICENSE
```

## License

MIT
