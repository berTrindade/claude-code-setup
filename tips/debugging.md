# Debugging Practices

Effective debugging strategies for Claude Code workflows.

## Screenshots for Visual Issues

When debugging UI or visual problems, take screenshots and compare states.

## Background Tasks for Logs

Run long-running processes in background to access logs:

```bash
tmux new-session -d -s dev "command"
tmux attach -t dev
```

## MCP for Console Logs

Use MCP servers to access application logs from databases, AWS, GitHub CI/CD.

## ASCII Diagrams

Use ASCII diagrams to visualize architecture, data flow, and debugging processes.

## Using /doctor

The `/doctor` command diagnoses common configuration and connection issues.

## Debugging Workflow

1. Reproduce the issue
2. Gather information (screenshots, logs, state)
3. Isolate the problem
4. Fix and verify

## Common Scenarios

- Command not working: Check command exists, verify frontmatter, check permissions
- Agent not responding: Verify agent file, check frontmatter, review logs
- Hook blocking unexpectedly: Check matcher pattern, review logic
- MCP server not connecting: Verify configuration, check credentials
