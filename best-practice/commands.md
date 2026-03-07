# Command Best Practices

Commands are entry-point prompts for workflows, invoked with `/command-name`.

## Frontmatter Template

All commands should include frontmatter:

```yaml
---
description: Brief description of what the command does
argument-hint: [optional-args]
allowed-tools: Read, Edit, Write, Bash, Grep, Glob
model: sonnet
---
```

## Example: start-ticket

```markdown
---
description: Start working on a ticket from the board. Fetches issue, spikes codebase, creates branch, produces plan.
argument-hint: [ticket-number]
allowed-tools: Read, Edit, Write, Bash, Grep, Glob
model: sonnet
---

Start working on ticket `<ticket-number>` from the board.

## Step 1 — Fetch Issue
...
```

## Best Practices

- **Clear description** - What does this command do?
- **Argument hints** - What arguments does it accept?
- **Explicit permissions** - List allowed tools explicitly
- **Model selection** - Choose appropriate model (sonnet default, opus for complex planning)
- **Step-by-step** - Break down into clear steps
- **Reference rules** - Link to `.claude/rules/` for conventions
