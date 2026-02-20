# Agent Instructions

## Overview
Example Cursor plugin demonstrating MCP servers, slash commands, skills, and rules.

## Commit Attribution
AI commits MUST include:
```
Co-Authored-By: (the agent model's name and attribution byline)
```

## MCP Server
- Config: `mcp.json` (root level, no dot prefix)
- Endpoint: `https://mcp.example.com/mcp`

## Plugin Structure
- `.cursor-plugin/plugin.json` — plugin metadata
- `.cursor-plugin/marketplace.json` — marketplace listing
- `mcp.json` — MCP server config
- `commands/` — slash commands
- `skills/` — agent skills
- `rules/` — coding rules and standards

## Skills
- **Code Review** — Analyze code quality, detect anti-patterns, and suggest improvements. See `skills/code-review/SKILL.md`
- **Setup Testing** — Configure test frameworks and generate test scaffolding. See `skills/setup-testing/SKILL.md`
- **Setup CI** — Configure CI/CD pipelines for GitHub Actions, GitLab CI, etc. See `skills/setup-ci/SKILL.md`
- **Refactor** — Identify and execute safe refactoring operations. See `skills/refactor/SKILL.md`
- **Setup Linting** — Configure ESLint, Prettier, Ruff, and other linters. See `skills/setup-linting/SKILL.md`

## Commands
- `/pulse` — Natural language queries about code quality and project health. See `commands/pulse.md`
- `/review` — Request an AI-powered code review of staged changes. See `commands/review.md`

## Rules
- `rules/code-quality.md` — Code quality standards and best practices
- `rules/testing-standards.md` — Testing conventions and coverage requirements
- `rules/api-design.md` — REST API design guidelines

## Key Conventions
- SKILL.md frontmatter: `name` and `description` only
- MCP config at root as `mcp.json` (not `.mcp.json`)
- Rules use markdown with frontmatter for metadata
