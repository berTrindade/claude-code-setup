# Workflow Patterns

Extracted workflow patterns for consistent execution across tickets and features.

## Command → Agent → Skill Orchestration Pattern

### Pattern Overview

The orchestration pattern separates concerns into three layers:

1. **Command** - Entry point, user interaction
   - Located in `.claude/commands/<name>.md`
   - Defines the workflow and user-facing interface
   - Delegates to agents for specialized tasks

2. **Agent** - Fetches data with preloaded skill (agent skill)
   - Located in `.claude/agents/<name>.md` or `~/.claude/agents/<name>.md`
   - Has domain-specific knowledge preloaded via `skills:` field
   - Uses MCP servers for external data access
   - Returns structured data to the command

3. **Skill** - Creates output independently (standalone skill)
   - Located in `.claude/skills/<name>/SKILL.md`
   - Can be invoked directly with `/skill-name`
   - Processes data and generates output
   - Reusable across multiple commands/agents

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

### Benefits

- **Separation of concerns** - Each layer has a clear responsibility
- **Reusability** - Skills can be used by multiple commands/agents
- **Testability** - Each component can be tested independently
- **Maintainability** - Changes to one layer don't affect others

## Command Patterns

### `/start-ticket <number>`
- Fetch GitHub issue and linked context
- Spike relevant codebase areas
- Identify risks (breaking changes, shared types, DB migrations, infra)
- **Infrastructure safety checks** (if applicable):
  - Check existing AWS resources
  - Verify Terraform S3 state backend
  - Ensure no accidental destruction/duplication
- Create branch from `develop` (ensure it's up to date first)
- Produce phased plan with exact file paths
- Wait for user confirmation

### `/verify`
- Build → typecheck → lint → tests → security scan
- Terraform validate (if infrastructure changed)
- Infrastructure pattern verification (if applicable)

### `/raise-pr`
- Run `/verify` first
- Review all changes
- Create PR following template
- Keep PR description simple

### `/bug-fix`
- Understand expected vs actual behavior
- Write failing test first
- Trace root cause
- Apply minimal fix
- Verify test passes, others still green

### `/hotfix`
- Branch from `master` (not `develop`)
- Reproduce locally
- Smallest fix only
- Run affected package tests only
- Open urgent PR to `master`

## Conventions Applied Automatically

### Git Workflow
- Branches from `develop` (except hotfixes from `master`)
- Conventional commits format
- **Reversible commits** — small, focused, organized by topic/domain
- See `.claude/rules/common/git-workflow.md`

### Code Style
- Comments follow style guidelines
- See `.claude/rules/code-style.md`

### Terraform Patterns
- Favor official Terraform Registry modules
- Naming: `{project}-{environment}-{component}`
- Tagging module applied to all resources
- Secrets: Flow A (ephemeral) or Flow B (external)
- IAM least-privilege
- See `.claude/rules/infrastructure/terraform-patterns.md`

### Terraform Style Guide
- Code formatting (`terraform fmt`)
- Block ordering (count/for_each first, tags last, lifecycle last)
- Naming conventions (lowercase with underscores)
- Variable/output descriptions required
- Prefer `for_each` over `count`
- See `.claude/rules/infrastructure/style-verification.md`

## Agent Delegation

| Agent | When Used |
|-------|-----------|
| `planner` | `/plan`, `/start-ticket` |
| `architect` | Architecture decisions, ADR creation |
| `code-reviewer` | `/raise-pr`, `/review-pr` |
| `security-reviewer` | Auth/input/API endpoint changes |
| `tdd-guide` | `/tdd`, `/bug-fix` |
| `database-reviewer` | SQL, migrations, schema design |
| `build-error-resolver` | Build or TypeScript failures |
| `refactor-cleaner` | `/refactor-clean` |

## MCP Server Usage

| Workflow | MCPs Used |
|----------|-----------|
| `/start-ticket` | `github`, `postgres`, `context7` |
| `/bug-fix` | `posthog`, `postgres`, `context7`, `playwright` |
| `/hotfix` | `posthog`, `postgres`, `aws` |
| `/review-pr` | `github`, `context7`, `aws-docs` |
| `/spike` | `postgres`, `posthog`, `sequential-thinking` |
| `/plan` | `sequential-thinking`, `github` |
| Infrastructure | `terraform`, `aws-docs`, `aws` |

## Plugins

Plugins extend Claude Code with additional tools and capabilities. Managed via `/plugins` command.

### Installed Plugins

| Plugin | Source | Purpose | Enabled |
|--------|--------|---------|---------|
| `typescript-lsp` | `claude-plugins-official` | Real-time TypeScript type checking, go-to-definition, completions | Yes |
| `mgrep` | `Mixedbread-Grep` | Semantic + keyword search, better than ripgrep for fuzzy codebase queries | Yes |

### Plugin Management

**Install from marketplace:**
```bash
claude plugin marketplace add <url>
claude plugin install <plugin-name>@<source>
```

**List installed plugins:**
```bash
claude plugin list
```

**Best Practices:**
- Keep under 10 enabled plugins (each consumes context window)
- Disable unused plugins to reduce context usage
- Use `/plugins` command to manage plugin lifecycle

### Plugin Discovery

- **Official plugins:** `claude-plugins-official` marketplace
- **Community plugins:** Add marketplace URL via `claude plugin marketplace add <url>`
- **Plugin conflicts:** If plugins conflict, disable one and test functionality

### Plugin Installation

1. Add marketplace (if not official): `claude plugin marketplace add <url>`
2. Install plugin: `claude plugin install <plugin-name>@<source>`
3. Verify installation: `claude plugin list`
4. Configure in `settings.local.json` if needed
