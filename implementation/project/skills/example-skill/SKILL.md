# Example Skill

This is an example skill structure for project-specific skills.

## Purpose

Skills provide reusable knowledge and workflows that can be:
- Preloaded into agents via the `skills:` field in agent frontmatter
- Invoked directly with `/skill-name` command
- Shared across multiple commands and agents

## Structure

```
.claude/skills/
└── example-skill/
    └── SKILL.md
```

## Usage

**In agent frontmatter:**
```yaml
---
skills: ["example-skill"]
---
```

**Direct invocation:**
```
/skill example-skill
```

## Best Practices

- Keep skills focused on a single domain or workflow
- Document clear usage patterns
- Include examples and common patterns
- Reference related skills and tools
