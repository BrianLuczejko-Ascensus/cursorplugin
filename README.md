# Cursor Plugin Demo

Example Cursor plugin demonstrating MCP servers, slash commands, skills, and rules. Use this repo to explore how Cursor plugins are structured.

## Installation

### From Local Source

```
git clone https://github.com/BrianLuczejko-Ascensus/cursorplugin.git
cd cursorplugin
```

Add the local directory as a plugin source in Cursor settings.

Restart Cursor to activate the plugin, then verify:

```
/help           # Should show /pulse and /review commands
/mcp            # Should show the MCP server
```

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

### code-review

Deep code quality analysis — finds anti-patterns, security issues, and maintainability problems.

```
Review the auth module for code quality issues
```

### refactor

Identifies and executes safe refactoring operations — extract functions, simplify conditionals, eliminate duplication.

```
Refactor src/services/payment.ts to reduce complexity
```

### Setup Skills

| Skill | Description |
|-------|-------------|
| `setup-testing` | Configure test frameworks (Jest, Vitest, pytest, RSpec) and generate test scaffolding |
| `setup-ci` | Setup CI/CD pipelines for GitHub Actions, GitLab CI, or CircleCI |
| `setup-linting` | Configure linters and formatters (ESLint, Prettier, Ruff, Biome) |

## Rules

The plugin ships with coding standard rules that the agent follows:

| Rule | Description |
|------|-------------|
| `code-quality` | Function complexity limits, naming conventions, error handling, security |
| `testing-standards` | Coverage requirements, test structure, mocking guidelines |
| `api-design` | REST API conventions, response codes, pagination, error formats |

## Configuration

The plugin automatically configures the MCP server on install. No additional setup required beyond restarting Cursor.

## Plugin Structure

```
cursorplugin/
├── .cursor-plugin/
│   ├── plugin.json           # Plugin metadata
│   └── marketplace.json      # Marketplace listing
├── mcp.json                  # MCP server configuration
├── AGENTS.md                 # Agent instructions
├── commands/
│   ├── pulse.md              # /pulse command
│   └── review.md             # /review command
├── skills/
│   ├── code-review/
│   ├── setup-testing/
│   ├── setup-ci/
│   ├── refactor/
│   └── setup-linting/
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
