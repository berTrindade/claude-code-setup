# CLI Reference

Command-line flags, utilities, and session management for Claude Code.

## Startup Flags

### Model Selection

```bash
claude --model sonnet    # Use Sonnet model
claude --model opus      # Use Opus model
claude --model haiku     # Use Haiku model
```

### Agent Selection

```bash
claude --agent code-reviewer    # Start with code-reviewer agent
claude --agent planner          # Start with planner agent
```

### Output Style

```bash
claude --output-style explanatory    # Detailed output with insights
claude --output-style concise        # Brief responses
```

### Environment Variables

```bash
export CLAUDE_TEAM_ID=feature-sprint-1    # Agent team coordination
export CLAUDE_MODEL=sonnet                 # Default model
export CLAUDE_OUTPUT_STYLE=explanatory     # Default output style
```

## Session Management

### `/rename`

Rename current session for easier identification.

```bash
/rename [TODO - refactor task]
```

**Use cases:**
- Mark important sessions
- Organize by task type
- Easy identification later

### `/resume`

Continue a previous session by name or ID.

```bash
/resume [session-name]
```

**Benefits:**
- Continue interrupted work
- Pick up where you left off
- Maintain context across sessions

### `/rewind` (Esc Esc)

Undo changes by rewinding to a previous checkpoint.

**When to use:**
- Claude goes off-track
- Unwanted changes made
- Better than trying to fix in same context

### `/compact`

Reduce context window usage by compacting conversation history.

**When to use:**
- Context approaching limits (use at max 50%)
- Switching to new task
- Reducing token usage

### `/clear`

Reset context mid-session.

**When to use:**
- Switching to completely new task
- Context becomes too cluttered
- Starting fresh is better than compacting

### `/fork`

Create a branch of the current conversation.

**Use cases:**
- Explore alternative approaches
- Test different solutions
- Keep original conversation intact

## Status Line

Customizable status bar showing context usage, model, cost, and session info.

### Configuration

In `settings.json`:
```json
{
  "statusline": {
    "enabled": true,
    "showContext": true,
    "showModel": true,
    "showCost": true
  }
}
```

### Benefits

- Real-time context awareness
- Fast compacting when needed
- Cost monitoring
- Model visibility

## Keybindings

Customize keybindings via `/keybindings` command or `~/.claude/keybindings.json`.

### Default Keybindings

- `Esc Esc` - Rewind to checkpoint
- Custom keybindings can be configured per user preference

### Configuration

Create `~/.claude/keybindings.json`:
```json
{
  "rewind": "Esc Esc",
  "compact": "Ctrl+C",
  "clear": "Ctrl+L"
}
```

## Spinner Verbs

Configure spinner verbs for personalized experience.

### Configuration

In `settings.json`:
```json
{
  "spinnerVerbs": ["thinking", "analyzing", "processing"]
}
```

### Purpose

- Visual feedback during processing
- Personalized experience
- Better UX during long operations

## Subcommands

### Plugin Management

```bash
claude plugin list                    # List installed plugins
claude plugin install <name>          # Install plugin
claude plugin marketplace add <url>   # Add marketplace
```

### MCP Management

```bash
claude mcp list       # List configured MCP servers
claude mcp test       # Test MCP server connections
```

### Configuration

```bash
claude config         # Open configuration
claude doctor         # Diagnose issues
```

## Best Practices

1. **Use `/rename`** for important sessions
2. **Use `/resume`** to continue work later
3. **Use `/rewind`** instead of trying to fix in same context
4. **Monitor `/usage`** to stay within plan limits
5. **Use `/compact`** at max 50% context usage
6. **Use `/clear`** when switching to completely new task
7. **Enable status line** for context awareness
8. **Configure keybindings** for faster workflows
