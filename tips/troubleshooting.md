# Troubleshooting

Common issues and solutions.

## MCP Server Not Available

1. Check if server is enabled in settings (not in `disabledMcpjsonServers`)
2. Verify prerequisites are installed (Docker, uv, etc.)
3. Check environment variables are set correctly
4. Verify network connectivity for remote MCPs

## Permission Errors

- Check `permissions.allow` in `settings.local.json` includes `mcp__*` patterns
- Verify agent has `mcpServers:` field in frontmatter if using agent-specific MCPs

## Connection Errors

- For `postgres`: Verify database is accessible and connection string is correct
- For `expo`: Ensure Expo dev server is running on `:8081`
- For `terraform`: Ensure Docker is running and can pull the image

## Compaction Errors

If you encounter compaction errors:
1. Use `/model` to select a 1M token model
2. Run `/compact` to reduce context
3. Continue with the larger context window

## Context Window Full

- Use `/compact` at max 50% context usage
- Use `/clear` to reset context mid-session if switching to a new task
- Disable unused plugins to reduce context
