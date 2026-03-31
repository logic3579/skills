# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A collection of reusable [Agent Skills](https://agentskills.io/specification), slash commands, and MCP server configurations for AI coding agents. Compatible with Claude Code, GitHub Copilot, OpenAI Codex, and other tools supporting the Agent Skills open standard.

This is a template/configuration repository — there is no build system, test framework, or compiled output.

## Conventions

- Each skill lives in its own directory under `skills/` with a `SKILL.md` file
- Skills follow the [Agent Skills specification](https://agentskills.io/specification)
- Skill names use lowercase with hyphens (e.g., `my-skill-name`)
- Slash commands go in `.claude/commands/` as `.md` files
- Sensitive data (credentials, API keys) must never be committed — use `.example` files as templates

### Git Commit Messages

All commits **MUST** follow the [Conventional Commits](https://www.conventionalcommits.org/) specification: `<type>[optional scope]: <description>`

Required types: `feat`, `fix`, `docs`, `chore`, `refactor`, `style`, `test`, `ci`, `perf`, `build`

Examples: `feat: add login page`, `fix: resolve parsing error`, `docs: update CLAUDE.md`, `chore: add MCP config`

## Architecture

```
skills/<skill-name>/
├── SKILL.md           # Skill definition with YAML frontmatter (required)
├── scripts/           # Executable scripts (optional)
├── references/        # Documentation, examples (optional)
└── assets/            # Output templates, files (optional)
```

- **SKILL.md** uses YAML frontmatter (`name`, `description`) followed by an `## Instructions` section with agent directives
- **`.claude/commands/`** contains slash command templates (pure markdown prompts, no executable code)
- **`.mcp.json.example`** is the MCP server config template for Claude Code; copy to `.mcp.json` and fill in real credentials
- **`.gemini/settings.json.example`** is the MCP config template for Gemini CLI; copy to `.gemini/settings.json` and fill in real credentials
- **`.codex/config.toml.example`** is the MCP config template for OpenAI Codex; copy to `.codex/config.toml` and fill in real credentials
- **`.mcp.json`**, **`.claude/settings.local.json`**, **`.gemini/settings.json`**, and **`.codex/config.toml`** are gitignored (contain secrets/local preferences)
- MCP servers use `stdio` transport: Claude Code communicates with child processes via stdin/stdout JSON-RPC
- Node.js MCP servers use `npx -y` for zero-install execution (e.g., kubernetes)
- Python MCP servers use `uvx` (alias for `uv tool run`) for zero-install execution (e.g., clickhouse), credentials are passed via `env`

## Creating a New Skill

Use the built-in `skill-creator` skill, or manually:

1. Create `skills/<skill-name>/SKILL.md` with frontmatter (`name`, `description`) and `## Instructions`
2. Add optional subdirectories (`scripts/`, `references/`, `assets/`) only if needed
3. Keep each skill focused and self-contained — one skill, one purpose
