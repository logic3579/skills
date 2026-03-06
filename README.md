# Skills

A collection of reusable [Agent Skills](https://agentskills.io/specification), slash commands, and MCP server configurations for AI coding agent CLI tools.

Compatible with **Claude Code**, **GitHub Copilot**, **OpenAI Codex**, and other tools that support the Agent Skills open standard.

## Structure

```
.
├── skills/                    # Agent Skills (SKILL.md standard)
│   └── <skill-name>/
│       ├── SKILL.md           # Skill definition (required)
│       ├── scripts/           # Executable scripts (optional)
│       ├── references/        # Documentation, examples (optional)
│       └── assets/            # Output templates, files (optional)
├── .claude/
│   ├── commands/              # Claude Code slash commands
│   └── settings.local.json   # Local settings (gitignored)
├── .mcp.json.example          # MCP server configuration template
├── CLAUDE.md                  # Project-level agent instructions
└── .gitignore
```

## Usage

### Install skills into your project

Copy the skill directories you need into your project's `skills/` or `.claude/skills/` directory:

```bash
cp -r skills/skill-name /path/to/your/project/.claude/skills/
```

### Set up MCP servers

```bash
cp .mcp.json.example .mcp.json
# Edit .mcp.json with your actual credentials
```

### Use slash commands

Copy commands into your project's `.claude/commands/` directory to enable them as `/command-name` in Claude Code.

## Adding a new skill

Each skill is a self-contained directory with a `SKILL.md` file:

```markdown
---
name: my-skill
description: Short description of what this skill does.
---

## Instructions

Detailed instructions for the agent...
```

## License

MIT
