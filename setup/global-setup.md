# Global Setup (`~/.claude/`)

Step-by-step guide to set up global Claude Code configuration.

## Step 1: Create Directory Structure

```bash
mkdir -p ~/.claude/{commands,agents,rules}
```

## Step 2: Create Global Settings

Create `~/.claude/settings.json` (see `implementation/global/settings.json.example`):

- Add `$schema` for IDE validation
- Add `plansDirectory` if custom plan location needed
- Add `attribution` settings for commits/PRs
- Add `statusline` configuration
- Add `alwaysThinkingEnabled` if desired
- Add `outputStyle` preferences (explanatory for Insight boxes)

## Step 3: Create Global CLAUDE.md

Create `~/.claude/CLAUDE.md` with your personal instructions (see `implementation/global/CLAUDE.md` for example).

## Step 4: Add Global Commands

Copy commands from `implementation/global/commands/` to `~/.claude/commands/`:
- `bug-fix.md`
- `hotfix.md`
- `review-pr.md`
- `spike.md`
- `verify.md`

## Step 5: Add Global Agents

Copy agents from `implementation/global/agents/` to `~/.claude/agents/`:
- `code-reviewer.md`
- `security-reviewer.md`
- `build-error-resolver.md`
- `e2e-runner.md`

## Step 6: Configure Global MCP Servers

Create `~/.claude/.mcp.json` (see `implementation/global/.mcp.json.example`):
- Replace all secrets with environment variables
- Configure GitHub, AWS, and other MCP servers
- See `best-practice/mcp-servers.md` for details

## Step 7: Set Environment Variables

Add to your `~/.zshrc` or `~/.bashrc`:

```bash
export GITHUB_PERSONAL_ACCESS_TOKEN="your-token"
export POSTHOG_PERSONAL_API_KEY="your-key"
# Add other required environment variables
```

## Step 8: Verify Setup

```bash
claude --version
claude mcp list
claude plugin list
```

## Next Steps

- See [project-setup.md](project-setup.md) for project-specific setup
- See [best-practice/](../best-practice/) for configuration patterns
