# GEMINI.md

This file provides instructions and context for Gemini CLI when working in this repository.

## Project Overview
This repository is a collection of reusable **Agent Skills**, slash commands, and **Model Context Protocol (MCP)** server configurations. It follows the [Agent Skills specification](https://agentskills.io/specification) and is designed to be compatible with various AI coding agents like Gemini CLI, Claude Code, and GitHub Copilot.

The project itself is a **Non-Code Project** (template/configuration repository). It does not have a build system or compiled output. Its primary purpose is to store and organize portable agent instructions and configurations.

## Directory Structure
- `skills/`: Contains individual agent skills, each in its own subdirectory.
  - Each skill directory must contain a `SKILL.md` file with YAML frontmatter (`name`, `description`) and an `## Instructions` section.
- `.gemini/settings.json`: Project-level configuration for Gemini CLI, including MCP server definitions.
- `.claude/commands/`: Markdown templates for slash commands (e.g., `/review`, `/test`).
- `.mcp.json.example`: A template for MCP server configurations.

## Key Files
- `CLAUDE.md`: General guidance for AI agents working in this repo.
- `.gemini/settings.json`: Contains the active MCP server configurations (e.g., `gcloud`, `kubernetes`, `clickhouse`).
- `skills/skill-creator/SKILL.md`: A meta-skill for creating new skills.

## Development Conventions
- **Skill Naming**: Use lowercase with hyphens (e.g., `my-new-skill`).
- **Self-Contained**: Each skill should be focused and reside in its own directory under `skills/`.
- **No Secrets**: Never commit sensitive data. Use `.example` files for templates and ensure `.gemini/settings.json` is handled carefully if it contains local overrides.
- **MCP Configuration**: MCP servers should be defined in `.gemini/settings.json` under the `mcpServers` key.
- **Git Commit Messages**: All commits **MUST** follow the [Conventional Commits](https://www.conventionalcommits.org/) specification: `<type>[optional scope]: <description>`. Required types: `feat`, `fix`, `docs`, `chore`, `refactor`, `style`, `test`, `ci`, `perf`, `build`. Examples: `feat: add login page`, `fix: resolve parsing error`, `docs: update CLAUDE.md`, `chore: add MCP config`.

## Common Tasks

### Creating a New Skill
To create a new skill, you should use the `skill-creator` logic:
1. Define a name (e.g., `git-helper`) and description.
2. Create `skills/git-helper/SKILL.md`.
3. Add an `## Instructions` section with specific agent directives.
4. (Optional) Add `scripts/`, `references/`, or `assets/` subdirectories if the skill requires them.

### Configuring MCP Servers
To add or update an MCP server for Gemini CLI:
1. Edit `.gemini/settings.json`.
2. Add a new entry to the `mcpServers` object following the standard MCP format (stdio, sse, or http).
3. Use `gemini mcp list` to verify the configuration.
4. If a server is disconnected, ensure the directory is trusted using `gemini trust`.

### Using Slash Commands
Slash commands in `.claude/commands/` are markdown-based prompts. To "run" them, read the corresponding `.md` file and follow the instructions within it.
