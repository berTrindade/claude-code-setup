# Advanced Features and Workflow Tips

Advanced Claude Code features and debugging practices.

## Scheduled Tasks (`/loop`)

Run prompts on a recurring schedule (up to 3 days), set one-time reminders, poll deployments and builds.

### Usage

```bash
/loop <interval> <prompt>
```

### Examples

- **Poll deployment:** `/loop 5m check if deployment is complete`
- **Monitor PR:** `/loop 10m check if PR has new comments`
- **One-time reminder:** `/loop 1h remind me to review the PR`

### Limitations

- Maximum duration: 3 days
- Interval: Minutes, hours, or days
- Auto-stops after completion or timeout

## Agent Teams

Multiple agents working in parallel on the same codebase with shared task coordination.

### Setup

1. **Use git worktrees** for isolation:
   ```bash
   git worktree add ../platform-feature-1 feature/1-new-feature
   git worktree add ../platform-feature-2 feature/2-another-feature
   ```

2. **Use tmux** for session management:
   ```bash
   tmux new-session -d -s agent1
   tmux new-session -d -s agent2
   ```

3. **Set team coordination** via environment variable:
   ```bash
   export CLAUDE_TEAM_ID=feature-sprint-1
   ```

### Best Practices

- Each agent works in its own worktree
- Use shared task coordination for related work
- Monitor agent progress independently
- Merge worktrees back when complete

## Thinking Mode & Output Styles

Configure reasoning visibility and output detail level.

### Configuration

**In settings.json:**
```json
{
  "alwaysThinkingEnabled": false,
  "outputStyle": "explanatory"
}
```

**Manual usage:**
- `/model` - Select model and reasoning level
- `/context` - See context usage
- `ultrathink` keyword - Request high-effort reasoning

### Output Styles

- **explanatory** - Detailed output with ★ Insight boxes (recommended)
- **concise** - Brief responses
- **thinking** - Show reasoning process

### Best Practices

- Use `outputStyle: "explanatory"` for better understanding
- Enable thinking mode for complex decisions
- Use `ultrathink` keyword when you need deep analysis

## Cross-Model Workflow

Use different models for different stages of work.

### Pattern

1. **Planning:** Use Opus (or higher model) for architecture decisions
2. **Implementation:** Use Sonnet for code writing
3. **Review:** Use Opus for comprehensive review

### Example

```bash
/model opus
# Create detailed plan

/model sonnet  
# Implement the plan

/model opus
# Review implementation
```

### When to Use

- Complex architecture decisions
- Critical code reviews
- Multi-phase projects
- When you need different reasoning capabilities

## Debugging Practices

### Screenshot Sharing

- Take screenshots of errors or UI issues
- Share with Claude for visual context
- Use `/image` or attach images to messages

### Background Tasks for Logs

- Run terminal commands as background tasks to capture logs
- Use `tmux` for persistent log sessions
- Example: `tmux new-session -d -s logs "npm run dev"`

### MCP for Console Logs

- Use Chrome MCP for browser console logs
- Use Playwright MCP for E2E test debugging
- Use Chrome DevTools MCP for detailed debugging

### ASCII Diagrams

- Use ASCII diagrams to explain architecture
- Helps Claude understand complex relationships
- Use mermaid syntax in markdown when possible

### Troubleshooting Commands

- `/doctor` - Diagnose installation, authentication, and configuration issues
- `/context` - See current context usage
- `/usage` - Check plan limits
- `/cost` - View token usage

### Compaction Errors

If you encounter compaction errors:
1. Use `/model` to select a 1M token model
2. Run `/compact` to reduce context
3. Continue with the larger context window

## Usage Monitoring

### Commands

- `/usage` - Check plan limits and current usage
- `/extra-usage` - Configure overflow billing
- `/cost` - View token usage and costs
- `/context` - See context window usage

### Rate Limits

- Be aware of rate limits for your plan
- Use `/usage` to monitor remaining quota
- Configure `/extra-usage` for pay-as-you-go overflow

### Cost Optimization

- Use smaller models (Haiku) for simple tasks
- Use `/compact` when context grows large
- Disable unused plugins to reduce context
- Use `/clear` to reset context when switching tasks
