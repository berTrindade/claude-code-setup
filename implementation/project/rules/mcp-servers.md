# MCP Servers Configuration and Usage

Model Context Protocol (MCP) servers provide Claude Code with access to external tools, databases, and APIs.

## Configuration Files

- **Global MCP servers:** `~/.claude/.mcp.json` - Available across all projects
- **Project MCP servers:** `.claude/.mcp.json` - Project-specific servers

## Global MCP Servers (`~/.claude/.mcp.json`)

| Server | Purpose | Authentication | Prerequisites |
|--------|---------|----------------|---------------|
| `github` | Read/write GitHub issues, PRs, repos | GitHub PAT in `GITHUB_PERSONAL_ACCESS_TOKEN` env var | None |
| `sequential-thinking` | Structured multi-step reasoning | None | None |
| `context7` | Live docs for React Native, Expo, Express, Terraform | None | None |
| `aws-docs` | AWS service documentation | None | `uv` installed |
| `aws` | Live AWS operations (Cognito, S3, CloudFront, Route53) | Local AWS credentials (`aws configure`) | AWS CLI configured |
| `playwright` | Browser automation for E2E debugging | None | None |
| `obsidian` | Personal notes vault access | None | Obsidian vault path configured |

## Project MCP Servers (`.claude/.mcp.json`)

| Server | Purpose | Authentication | Prerequisites |
|--------|---------|----------------|---------------|
| `postgres` | Query PostgreSQL database directly | Database connection string in args | `@zeddotdev/postgres-context-server` |
| `posthog` | Analytics, feature flags, SQL insights | PostHog Personal API key in `POSTHOG_PERSONAL_API_KEY` env var | None |
| `terraform` | Terraform Registry provider/resource/module docs | None | Docker installed |
| `expo` | Live Expo dev server (bundle info, logs, device state) | None | Expo dev server running on `:8081` |
| `playwright` | Run and debug platform E2E tests | None | None |

## MCP Server Usage by Workflow

| Workflow | MCPs Used | Purpose |
|----------|-----------|---------|
| `/start-ticket` | `github`, `postgres`, `context7` | Fetch issue, check schema, get framework docs |
| `/bug-fix` | `posthog`, `postgres`, `context7`, `playwright` | Error scope, data state, framework docs, E2E repro |
| `/hotfix` | `posthog`, `postgres`, `aws` | Blast radius, data check, live infra state |
| `/review-pr` | `github`, `context7`, `aws-docs` | PR data, framework APIs, infra resource docs |
| `/spike` | `postgres`, `posthog`, `sequential-thinking` | Database queries, usage patterns, structured reasoning |
| `/plan` | `sequential-thinking`, `github` | Multi-step planning, issue context |
| `/raise-pr` | `github` | Create PR, update issue |
| Mobile (Expo/RN) | `expo`, `playwright`, `context7` | Dev server, E2E tests, RN/Expo docs |
| Terraform/AWS infra | `terraform`, `aws-docs`, `aws` | Registry schemas, service docs, live state |

## Security Best Practices

### Secrets Management

**Never commit secrets to version control:**

1. **Use environment variables** for all sensitive values:
   ```json
   {
     "env": {
       "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}",
       "POSTHOG_PERSONAL_API_KEY": "${POSTHOG_API_KEY}"
     }
   }
   ```

2. **Use placeholders** in committed config files:
   ```json
   {
     "args": ["-y", "@zeddotdev/postgres-context-server", "${DATABASE_URL}"]
   }
   ```

3. **Set environment variables** in your shell or `.env` file (not committed)

### Permission Rules

MCP tools use the `mcp__*` syntax in permissions:
- `mcp__github:read_issue` - Read GitHub issues
- `mcp__postgres:query` - Query PostgreSQL database
- `mcp__aws:describe_resources` - Describe AWS resources

## MCP Server Scopes

- **Global (`~/.claude/.mcp.json`):** Available to all projects and agents
- **Project (`.claude/.mcp.json`):** Only available within this project
- **Subagent:** Can specify `mcpServers:` in agent frontmatter for agent-specific MCPs

## Setup Checklist

### Credentials

- [ ] **GitHub PAT** - Set `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable
- [ ] **PostHog API Key** - Set `POSTHOG_PERSONAL_API_KEY` environment variable  
- [ ] **Database URL** - Replace `YOUR_DATABASE_URL_HERE` in `.claude/.mcp.json` with actual connection string
- [ ] **AWS Credentials** - Configure via `aws configure` (used by `aws` MCP)

### Runtime Prerequisites

- [ ] **Docker** - Required for `terraform` MCP server
- [ ] **uv** - Required for `aws-docs` MCP server (`pip install uv`)
- [ ] **Expo Dev Server** - Must be running on `:8081` for `expo` MCP (`cd platform-app && npx expo start`)

## Troubleshooting

### MCP Server Not Available

1. Check if server is enabled in settings (not in `disabledMcpjsonServers`)
2. Verify prerequisites are installed (Docker, uv, etc.)
3. Check environment variables are set correctly
4. Verify network connectivity for remote MCPs

### Permission Errors

- Check `permissions.allow` in `settings.local.json` includes `mcp__*` patterns
- Verify agent has `mcpServers:` field in frontmatter if using agent-specific MCPs

### Connection Errors

- For `postgres`: Verify database is accessible and connection string is correct
- For `expo`: Ensure Expo dev server is running on `:8081`
- For `terraform`: Ensure Docker is running and can pull the image
