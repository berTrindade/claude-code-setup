# Rules Overview

This directory contains project-specific rules and guidelines for Claude Code.

## Directory Structure

- **`common/`** - Common rules used across all projects
  - `git-workflow.md` - Git branching, commit message conventions, PR workflow
  - `code-style.md` - Code style guidelines, comment conventions

- **`infrastructure/`** - Infrastructure-specific rules
  - `terraform-patterns.md` - AWS/Terraform patterns and best practices
  - `secrets-management.md` - Secrets management patterns (Flow A & Flow B)
  - `style-verification.md` - Terraform style guide verification checklist
  - `workflow-examples.md` - Examples of infrastructure workflows

- **`workflows.md`** - Workflow patterns and orchestration patterns
- **`mcp-servers.md`** - MCP server configuration and usage
- **`built-in-features.md`** - Checkpointing, voice mode, remote control, git worktrees
- **`advanced-features.md`** - Scheduled tasks, agent teams, thinking mode, debugging
- **`cli-reference.md`** - CLI flags, session management, utilities

## Usage

Rules are automatically loaded by Claude Code when referenced in:
- `CLAUDE.local.md` files
- Command files
- Agent definitions

Use `@path` imports in CLAUDE.md files to reference specific rules:

```markdown
See `.claude/rules/infrastructure/terraform-patterns.md` for Terraform patterns.
```

## Adding New Rules

1. Create the rule file in the appropriate subdirectory
2. Update this README to include the new rule
3. Reference it in relevant commands, agents, or CLAUDE.md files
