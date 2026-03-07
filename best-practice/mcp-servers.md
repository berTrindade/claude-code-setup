# MCP Server Best Practices

Model Context Protocol (MCP) servers provide Claude Code with access to external tools, databases, and APIs.

## Configuration Files

- **Global:** `~/.claude/.mcp.json` - Available across all projects
- **Project:** `.claude/.mcp.json` - Project-specific servers

## Security Best Practices

### Never Commit Secrets

**Use environment variables:**

```json
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**Set in your shell:**

```bash
export GITHUB_PERSONAL_ACCESS_TOKEN="your-token-here"
```

### Sanitization Example

```json
// ❌ BAD - Never commit this
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_abc123..."
      }
    }
  }
}

// ✅ GOOD - Use placeholders
{
  "mcpServers": {
    "github": {
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

## Common MCP Servers

### GitHub

```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
    }
  }
}
```

### PostgreSQL

```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@zeddotdev/postgres-context-server", "${DATABASE_URL}"]
  }
}
```

### Terraform Registry

```json
{
  "terraform": {
    "command": "docker",
    "args": ["run", "--rm", "-i", "hashicorp/terraform-mcp-server:latest"]
  }
}
```

## Best Practices

- **Use environment variables** for all secrets
- **Document prerequisites** (Docker, uv, etc.)
- **Scope appropriately** - Global vs project vs agent-specific
- **Document usage** - Which workflows use which MCPs
- **Test connections** - Verify MCPs work before committing
