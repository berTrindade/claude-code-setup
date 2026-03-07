# Ticket Workflow

How to work on a ticket using `/start-ticket <number>`.

## Overview

When you start working on a ticket, Claude automatically:

1. Fetches GitHub issue and linked context
2. Spikes relevant codebase areas
3. Identifies risks (breaking changes, shared types, DB migrations, infra)
4. Performs infrastructure safety checks (if applicable)
5. Creates branch from `develop`
6. Produces phased plan
7. Waits for your confirmation

## Infrastructure Safety Checks

If infrastructure changes are involved:

1. **Check existing AWS resources** for the target environment
2. **Verify Terraform S3 state backend** is accessible and working correctly
3. **Ensure no resources will be accidentally destroyed or duplicated**
4. **Apply Terraform patterns automatically:**
   - Favor official Terraform Registry modules over custom code
   - Use naming convention: `{project}-{environment}-{component}`
   - Apply tagging module for standard tags
   - Follow Flow A (ephemeral) for DB secrets or Flow B (external) for API keys
   - Implement least-privilege IAM policies

## Conventions Applied Automatically

- Branches created from `develop` (see `.claude/rules/common/git-workflow.md`)
- Commit messages use conventional commits format
- **Commits must be reversible** — small, focused commits organized by topic/domain
- Code comments follow style guidelines
- PR descriptions follow template and should be simple
- Terraform patterns (see `.claude/rules/infrastructure/terraform-patterns.md`)

## Example Usage

```bash
# Start working on ticket 22
/start-ticket 22

# Claude will:
# 1. Fetch issue #22
# 2. Explore codebase
# 3. Identify risks
# 4. Create branch: feature/22-slug
# 5. Present plan
# 6. Wait for your go-ahead
```
