# Skills Best Practices

Skills are reusable knowledge and workflows that can be invoked directly or preloaded into agents.

## Skill Structure

Skills are located in `.claude/skills/<name>/SKILL.md` and follow a standard structure:

```markdown
# Skill Name

Brief description of what this skill does.

## When to Use This Skill

**Activate this skill when:**
- Specific use case 1
- Specific use case 2

**Don't use this skill for:**
- Anti-pattern or misuse case

## Core Concepts

Main concepts and patterns this skill teaches.

## Examples

Practical examples of using this skill.

## Reference

Links to external documentation or related skills.
```

## Skill Types

### 1. Standalone Skills

Skills that can be invoked directly with `/skill-name`:

- Located in `.claude/skills/<name>/SKILL.md`
- Self-contained knowledge and workflows
- Can be used independently or by commands/agents

**Example:** `/terraform-style-guide`, `/terraform-skill`

### 2. Agent Skills (Preloaded)

Skills preloaded into agents via the `skills:` field in agent frontmatter:

```yaml
---
name: code-reviewer
skills: ["terraform-style-guide", "terraform-skill"]
---
```

**Benefits:**
- Agents have domain knowledge without explicit invocation
- Faster execution (no need to load skill on-demand)
- Consistent application across agent interactions

## Best Practices

### Skill Organization

- **One skill per domain** - Keep skills focused and single-purpose
- **Clear naming** - Use descriptive, lowercase names with hyphens
- **Progressive disclosure** - Essential info in main file, details in references

### Skill Content

- **When to use** - Clear activation criteria
- **Examples** - Practical, real-world examples
- **Anti-patterns** - What NOT to do
- **References** - Links to related skills or documentation

### Skill Usage

- **Preload in agents** - For domain-specific agents (e.g., terraform skills in infrastructure agents)
- **Invoke directly** - For ad-hoc use or when not preloaded
- **Combine skills** - Multiple skills can work together

## Example: Terraform Skills

```yaml
# Agent with preloaded Terraform skills
---
name: infrastructure-reviewer
skills: ["terraform-style-guide", "terraform-skill"]
tools: ["Read", "Grep", "Bash"]
---
```

When this agent reviews infrastructure code, it automatically applies:
- HashiCorp style guide conventions
- Testing strategies and module patterns
- Security best practices
- CI/CD patterns

## Creating New Skills

1. **Create directory:** `.claude/skills/<skill-name>/`
2. **Create SKILL.md** with standard structure
3. **Add to agent** via `skills:` field if needed
4. **Document usage** in workflow documentation

## Skill Discovery

- **Global skills:** `~/.claude/skills/` - Available across all projects
- **Project skills:** `.claude/skills/` - Project-specific skills
- **Plugin skills:** Installed via `/plugins` command

## Tips

- Keep skills focused and maintainable
- Update skills as patterns evolve
- Share useful skills across projects
- Document skill dependencies and prerequisites
