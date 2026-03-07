# Project Setup (`.claude/`)

Step-by-step guide to set up project-specific Claude Code configuration.

## Step 1: Create Directory Structure

```bash
mkdir -p .claude/{commands,agents,rules/{common,infrastructure},skills,hooks/{pre-tool-use,post-tool-use,stop}}
```

## Step 2: Create Project Settings

Create `.claude/settings.local.json` (see `implementation/project/settings.local.json.example`):

- Add `$schema` for IDE validation
- Add `plansDirectory` if custom plan location needed
- Add `attribution` settings for commits/PRs
- Add `statusline` configuration
- Add `alwaysThinkingEnabled` if desired
- Add `outputStyle` preferences (explanatory for Insight boxes)

## Step 3: Create Project Commands

Copy commands from `implementation/project/commands/`:
- `start-ticket.md`
- `raise-pr.md`
- `plan.md`
- `tdd.md`
- `refactor-clean.md`

## Step 4: Create Rules

Copy rules from `implementation/project/rules/`:
- `common/git-workflow.md`
- `common/code-style.md`
- `infrastructure/terraform-patterns.md`
- `infrastructure/secrets-management.md`
- `infrastructure/style-verification.md`
- `workflows.md`
- `mcp-servers.md`
- `built-in-features.md`
- `advanced-features.md`
- `cli-reference.md`
- `README.md`

## Step 5: Create Hooks

Copy hooks from `implementation/project/hooks/`:
- `pre-tool-use/dev-server-block.md`
- `pre-tool-use/git-push-reminder.md`
- `post-tool-use/prettier-format.md`
- `stop/console-log-check.md`

## Step 6: Create WORKFLOWS.md

Copy `implementation/project/WORKFLOWS.md` to `.claude/WORKFLOWS.md` (or create based on your project needs)

## Step 7: Configure Project MCP Servers

Create `.claude/.mcp.json` (see `implementation/project/.mcp.json.example`):
- Replace all secrets with environment variables
- Configure project-specific MCPs (postgres, terraform, expo, etc.)

## Step 8: Create CLAUDE.local.md

Create `.claude/CLAUDE.local.md` with project-specific instructions (see `implementation/project/CLAUDE.local.md` for example).

## Step 9: Verify Setup

```bash
claude --version
# Test a command
claude /start-ticket 1
```

## Customization

- Adapt commands to your project's needs
- Add project-specific agents
- Configure project-specific MCP servers
- Customize rules for your tech stack

## Next Steps

- See [best-practice/](../best-practice/) for configuration patterns
- See [workflows/](../workflows/) for workflow examples
