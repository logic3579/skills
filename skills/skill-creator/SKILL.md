---
name: skill-creator
description: Create a new Agent Skill with proper SKILL.md structure and supporting files.
---

## Instructions

When the user asks to create a new skill, follow these steps:

1. Ask for the skill name (lowercase, hyphens) and a short description if not provided
2. Create a new directory under `skills/<skill-name>/`
3. Generate a `SKILL.md` file with the following structure:

```markdown
---
name: <skill-name>
description: <Short description of what this skill does.>
---

## Instructions

<Detailed instructions for the agent>
```

4. If the skill needs scripts, create a `scripts/` subdirectory
5. If the skill needs reference docs, create a `references/` subdirectory
6. If the skill produces files from templates, create an `assets/` subdirectory

Keep skills focused and self-contained. Each skill should do one thing well.
