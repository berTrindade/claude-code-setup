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

## Available Commands

### Global Commands (`~/.claude/commands/`)

See `implementation/global/commands/` for examples:
- `bug-fix.md` - Investigate and fix bugs
- `hotfix.md` - Fast path for production issues
- `review-pr.md` - Review teammate PRs
- `spike.md` - Pure research and investigation
- `verify.md` - Full pre-PR verification

### Project Commands (`.claude/commands/`)

See `implementation/project/commands/` for examples:
- `bug-fix.md` - Investigate and fix bugs
- `hotfix.md` - Production hotfix workflow
- `plan.md` - Create phased implementation plan
- `raise-pr.md` - Create PR following template
- `refactor-clean.md` - Clean up dead code
- `review-pr.md` - Review PRs
- `spike.md` - Research and investigation
- `start-ticket.md` - Start working on a ticket
- `tdd.md` - TDD workflow
- `verify.md` - Pre-PR verification
