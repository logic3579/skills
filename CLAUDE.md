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
- **`.mcp.json.example`** is the MCP server config template; copy to `.mcp.json` and fill in real credentials
- **`.mcp.json`** and **`.claude/settings.local.json`** are gitignored (contain secrets/local preferences)
- MCP servers use `stdio` transport: Claude Code communicates with child processes via stdin/stdout JSON-RPC
- Node.js MCP servers use `npx -y` for zero-install execution (e.g., kubernetes)
- Python MCP servers use `uvx` (alias for `uv tool run`) for zero-install execution (e.g., clickhouse), credentials are passed via `env`

## Creating a New Skill

Use the built-in `skill-creator` skill, or manually:

1. Create `skills/<skill-name>/SKILL.md` with frontmatter (`name`, `description`) and `## Instructions`
2. Add optional subdirectories (`scripts/`, `references/`, `assets/`) only if needed
3. Keep each skill focused and self-contained — one skill, one purpose
