# Repository Guidelines

## Project Structure & Module Organization
This repository is a template and collection of Agent Skills, slash commands, and MCP configuration examples. Core locations:

- `skills/<skill-name>/SKILL.md`: skill definition with YAML frontmatter and `## Instructions`.
- `skills/<skill-name>/scripts/`, `references/`, `assets/`: optional, skill-scoped supporting files.
- `.claude/commands/`: Claude Code slash command templates (`.md`).
- `.mcp.json.example`: MCP server config template (copy to `.mcp.json` locally).
- `CLAUDE.md`: project-level agent instructions and architecture notes.

## Build, Test, and Development Commands
There is no build system or test runner in this repository. Typical tasks are file edits and copying templates.

- Copy a skill into a consumer project:
  ```bash
  cp -r skills/skill-name /path/to/project/.claude/skills/
  ```
- Initialize MCP config locally:
  ```bash
  cp .mcp.json.example .mcp.json
  ```

## Coding Style & Naming Conventions
- Skill directory names use lowercase with hyphens, e.g., `my-skill-name`.
- `SKILL.md` files use YAML frontmatter with `name` and `description`, followed by `## Instructions`.
- Keep Markdown concise; prefer short sections and actionable steps.
- No formatter or linter is enforced; match existing style in nearby files.

## Testing Guidelines
No automated tests are currently defined. Validate changes by:
- Ensuring `SKILL.md` frontmatter parses cleanly.
- Confirming referenced paths exist (scripts, references, assets).

## Commit & Pull Request Guidelines
Recent commit history uses short, imperative summaries like “Add …”, “Update …”, “Initialize …”. Follow the same style.

For pull requests:
- Describe the new skill or change scope clearly.
- Link relevant issues or specs if available.
- Include examples or screenshots when adding new command templates or complex prompts.

## Security & Configuration Tips
- Do not commit secrets. Use `.mcp.json.example` as a template and keep `.mcp.json` local.
- Avoid embedding credentials in `SKILL.md` or command templates.
