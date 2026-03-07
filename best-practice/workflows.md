# Workflow Best Practices

Workflow patterns for consistent execution across tickets and features.

## Command → Agent → Skill Pattern

### Pattern Overview

1. **Command** - Entry point, user interaction (`.claude/commands/`)
2. **Agent** - Fetches data with preloaded skill (`.claude/agents/` or `~/.claude/agents/`)
3. **Skill** - Creates output independently (`.claude/skills/<name>/SKILL.md`)

### Example Flow

```
User: /start-ticket 123
  ↓
Command: start-ticket.md
  ↓
Agent: planner (with planning skill preloaded)
  ↓
  - Uses github MCP to fetch issue
  - Uses postgres MCP to check schema
  - Uses context7 MCP for framework docs
  ↓
Skill: ticket-analysis (standalone)
  ↓
  - Analyzes issue requirements
  - Generates implementation plan
  ↓
Command: Presents plan to user
```

## Ticket Workflow

### `/start-ticket <number>`

1. Fetch GitHub issue and linked context
2. Spike relevant codebase areas
3. Identify risks (breaking changes, shared types, DB migrations, infra)
4. **Infrastructure safety checks** (if applicable):
   - Check existing AWS resources
   - Verify Terraform S3 state backend
   - Ensure no accidental destruction/duplication
5. Create branch from `develop` (ensure it's up to date first)
6. Produce phased plan with exact file paths
7. Wait for user confirmation

## Conventions Applied Automatically

- Branches from `develop` (except hotfixes from `master`)
- Conventional commits format
- **Reversible commits** — small, focused, organized by topic/domain
- Code comments follow style guidelines
- PR descriptions follow template and should be simple
- Terraform patterns (favor registry modules, naming conventions, etc.)
