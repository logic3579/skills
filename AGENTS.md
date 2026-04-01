# AGENTS.md - Repository Guidelines

This file provides guidance for AI coding agents (Claude Code, OpenCode, Gemini CLI, GitHub Copilot, Codex) operating in this repository.

## Project Overview

This repository is a template and collection of **Agent Skills**, slash commands, and **MCP (Model Context Protocol)** server configurations. It follows the [Agent Skills specification](https://agentskills.io/specification) and is designed to be compatible with various AI coding agents.

This is a **non-code project** — there is no build system, test framework, or compiled output. Its primary purpose is storing and organizing portable agent instructions and configurations.

---

## Project Structure

```
.
├── skills/                    # Agent skills, each in its own directory
│   └── <skill-name>/
│       ├── SKILL.md           # Skill definition (required)
│       ├── scripts/           # Executable scripts (optional)
│       ├── references/       # Documentation, examples (optional)
│       └── assets/            # Output templates, files (optional)
├── .claude/
│   ├── commands/              # Slash command templates (.md files)
│   └── settings.local.json    # Local settings (gitignored)
├── .codex/
│   └── config.toml            # Codex MCP config (no secrets, env inheritance)
├── .gemini/
│   └── settings.json          # Gemini MCP config (no secrets, env inheritance)
├── .opencode/
│   └── opencode.json          # OpenCode config with plugins & MCP (no secrets, env inheritance)
├── .mcp.json                  # Claude Code MCP config (no secrets, env inheritance)
├── CLAUDE.md                  # Claude Code guidance
├── GEMINI.md                  # Gemini CLI guidance
└── AGENTS.md                  # This file
```

---

## Build, Test, and Development Commands

There is **no build system or test runner** in this repository. Typical tasks involve file edits, creating templates, and copying skills to consumer projects.

### Common Tasks

| Task | Command |
|------|---------|
| Copy a skill to a consumer project | `cp -r skills/skill-name /path/to/project/.claude/skills/` |
| Set up MCP credentials | Export `CLICKHOUSE_HOST`, `CLICKHOUSE_USER`, `CLICKHOUSE_PASSWORD` in shell profile |
| Verify SKILL.md YAML frontmatter | Manual check — ensure `name` and `description` fields are present |

### Validation Checklist

Before submitting changes:
- [ ] `SKILL.md` frontmatter parses cleanly (valid YAML with `name`, `description`)
- [ ] All referenced paths exist (scripts, references, assets)
- [ ] No secrets or credentials embedded in SKILL.md or command templates
- [ ] Follows naming conventions (lowercase with hyphens)

---

## Code Style Guidelines

Since this is a configuration/template repository, "code" primarily refers to SKILL.md files, shell scripts, and JSON/YAML configurations.

### File Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Skill directories | lowercase with hyphens | `git-helper`, `code-reviewer` |
| SKILL.md files | Exactly `SKILL.md` | `skills/git-helper/SKILL.md` |
| Slash commands | lowercase, descriptive | `.claude/commands/test.md` |
| Scripts | lowercase with hyphens or snake_case | `scripts/deploy.sh` |
| MCP config (Claude Code) | `.mcp.json` | Committed (no secrets) |
| MCP config (Gemini CLI) | `.gemini/settings.json` | Committed (no secrets) |
| MCP config (Codex) | `.codex/config.toml` | Committed (no secrets) |
| Config (OpenCode) | `.opencode/opencode.json` | Committed (no secrets) |

### SKILL.md Format

Every skill **must** have a `SKILL.md` file with:

```markdown
---
name: <skill-name>
description: <Short description (1-2 sentences)>
---

## Instructions

<Detailed instructions for the agent>
```

**YAML Frontmatter Requirements:**
- `name`: lowercase with hyphens, matches directory name
- `description`: concise, action-oriented summary

**Instructions Section:**
- Use `## Instructions` as the header (exact, not `###` or other variants)
- Keep instructions focused and actionable
- Use numbered lists for sequential steps
- Use bullet points for optional items or examples

### Markdown Style

- Keep Markdown concise; prefer short sections and actionable steps
- Use code blocks with language hints for examples
- Use tables for structured information (configs, parameters)
- Avoid excessive nesting (prefer 2-3 levels max)
- No formatter or linter enforced — match existing style in nearby files

### Shell Script Conventions (scripts/)

- Include shebang (`#!/bin/bash` or `#!/usr/bin/env bash`)
- Use `set -e` for fail-fast behavior
- Add comments only for non-obvious logic
- Name variables descriptively (`SKILL_DIR` not `SD`)
- Use `[[ ]]` for conditionals (bash)

### JSON/YAML Conventions

- Use 2-space indentation
- Trailing commas are allowed in JSON (JavaScript-style)
- Use comments in JSONc where supported
- Quote strings only when necessary (keys are usually unquoted)

---

## Import & Dependency Guidelines

This repository does not use external package managers (npm, pip, etc.) for the templates themselves. However:

- **Skills may reference external tools** (e.g., `eslint`, `prettier`) — document these in SKILL.md
- **MCP servers** are defined in `.mcp.json` or `.opencode/opencode.json` and may require local installation
- **OpenCode plugins** are installed automatically via Bun when the config changes (dependencies stored in `.opencode/node_modules/`, gitignored)
- **Scripts** should be self-contained or document required dependencies

---

## Error Handling

### SKILL.md Validation Errors

If a skill's SKILL.md has issues:
1. Report the specific YAML parsing error
2. Suggest the correct format with an example
3. Do not attempt to "fix" by guessing — ask the user

### MCP Configuration Errors

- If an MCP server fails to connect, check:
  - Server is installed (`which <server>`)
  - Path is correct in `.mcp.json` or `.opencode/opencode.json`
  - Required environment variables are set

### Script Errors

- Scripts should fail fast with clear error messages
- Use `exit 1` for errors
- Print helpful context before exiting

---

## Git & Version Control

### Commit Message Style

All commits **MUST** follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Required types:**

| Type | Usage |
|------|-------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `chore` | Maintenance, configs, dependencies |
| `refactor` | Code restructuring without behavior change |
| `style` | Formatting, whitespace (no logic change) |
| `test` | Adding or updating tests |
| `ci` | CI/CD configuration changes |
| `perf` | Performance improvements |
| `build` | Build system or external dependency changes |

**Rules:**
- Type is **required**, scope is optional
- Description must be lowercase, imperative mood, no period at the end
- ✅ `feat: add git-helper skill`
- ✅ `docs: update mcp config template`
- ✅ `fix: resolve skill-creator parsing error`
- ✅ `chore: add gcloud MCP server configuration`
- ❌ `Added a new skill` (missing type, past tense)
- ❌ `Fixed stuff.` (missing type, vague, has period)
- ❌ `feat:Add feature` (missing space after colon)

### Branch Naming

- Feature: `skill/<skill-name>` or `add/<description>`
- Fix: `fix/<issue-description>`
- Config: `update/mcp-config` or similar

### Pull Request Guidelines

- Describe the new skill or change scope clearly
- Link relevant issues or specs if available
- Include examples or screenshots for complex prompts
- Ensure no sensitive data (credentials, API keys) is hardcoded in config files

---

## Security & Configuration Tips

### Never Commit Secrets

- **Do not** hardcode credentials, API keys, or tokens in config files
- Config files (`.mcp.json`, `.gemini/settings.json`, `.codex/config.toml`, `.opencode/opencode.json`) are committed to the repo with sensitive fields omitted
- Sensitive values (host, user, password) are inherited from shell environment variables at runtime
- Keep `.claude/settings.local.json` gitignored (local preferences)
- Warn user if they accidentally include secrets

### Skill Security

- Avoid embedding credentials in `SKILL.md` or command templates
- Document required environment variables clearly
- Use placeholder values (e.g., `YOUR_API_KEY`) in examples

---

## Working with AI Agents

### Claude Code (CLAUDE.md)

- Follow conventions in `CLAUDE.md`
- Use `.claude/commands/` for slash command templates
- Check `.claude/settings.local.json` for agent preferences

### OpenCode

- Follow conventions in AGENTS.md (this file)
- Skills work out of the box with OpenCode's skill system
- Use `/test` or `/review` slash commands from `.claude/commands/`
- MCP servers are defined in `.opencode/opencode.json`
- Plugins are configured in the `plugin` array of `.opencode/opencode.json`
- Use `opencode debug config` to verify configurations
- Use `opencode debug skill` to list all available skills

### Gemini CLI

- Follow conventions in `GEMINI.md`
- MCP servers are defined in `.gemini/settings.json`
- Use `gemini mcp list` to verify configurations

### GitHub Copilot

- This repo is primarily for Copilot **extensions** and **custom instructions**
- No special Copilot rules found in `.github/copilot-instructions.md`

---

## Quick Reference

| Task | Location |
|------|----------|
| Create new skill | `skills/<skill-name>/SKILL.md` |
| Add slash command | `.claude/commands/<name>.md` |
| Configure MCP (Claude Code) | `.mcp.json` |
| Configure MCP (Gemini CLI) | `.gemini/settings.json` |
| Configure MCP (Codex) | `.codex/config.toml` |
| Configure (OpenCode) | `.opencode/opencode.json` |
| Agent-specific config | `CLAUDE.md`, `GEMINI.md`, or `.claude/settings.local.json` |
| Project overview | `README.md` |

---

## Contact & Support

- For issues with skills: open an issue in the repository
- For MCP server problems: check the respective server's documentation
- For agent-specific questions: refer to `CLAUDE.md` or `GEMINI.md`
