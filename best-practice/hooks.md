# Hooks Best Practices

Hooks are deterministic scripts that run automatically on specific events (pre-tool-use, post-tool-use, stop).

## Hook Structure

Hooks are organized in `.claude/hooks/` directory:

```
.claude/hooks/
├── pre-tool-use/     # Run before tool execution
│   ├── dev-server-block.md
│   └── git-push-reminder.md
├── post-tool-use/    # Run after tool execution
│   └── prettier-format.md
└── stop/             # Run when session stops
    └── console-log-check.md
```

## Hook File Format

Each hook file contains:

```markdown
# Hook Name

Brief description of what this hook does.

**Matcher:** `ToolName` or pattern
**Hook Type:** PreToolUse | PostToolUse | Stop
**Command:**
```javascript
// JavaScript code that processes tool input/output
```

**Purpose:** Why this hook exists and what problem it solves.
```

## Hook Types

### PreToolUse Hooks

Run **before** a tool executes. Can block execution or modify input.

**Use cases:**
- Block dangerous commands (e.g., dev servers outside tmux)
- Validate inputs before execution
- Add reminders or warnings

### PostToolUse Hooks

Run **after** a tool executes. Can process output or trigger follow-up actions.

**Use cases:**
- Auto-format code after edits
- Run linters after file changes
- Send notifications after deployments

### Stop Hooks

Run when the session stops. Useful for cleanup or final checks.

**Use cases:**
- Check for console.log statements
- Validate commit messages
- Generate session summaries

## Best Practices

- **Deterministic** - Same input always produces same output
- **Fast** - Hooks run synchronously, keep them quick
- **Focused** - One hook, one purpose
- **Non-blocking** - Use warnings instead of blocking when possible
