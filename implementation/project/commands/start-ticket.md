---
description: Start working on a ticket from the board. Fetches issue, spikes codebase, creates branch, produces plan.
argument-hint: [ticket-number]
allowed-tools: Read, Edit, Write, Bash, Grep, Glob
model: sonnet
---

Start working on ticket `<ticket-number>` from the board.

## Step 1 — Fetch Issue

```bash
gh issue view <ticket-number>
```

Understand:
- What is the ticket asking for?
- What issue does it close?
- Are there any linked PRs or related issues?

## Step 2 — Spike Relevant Code

Explore the codebase following the data flow.

Identify:
- Which files/modules are involved?
- What are the key functions, types, and interfaces?
- Where are the edges — dependencies and dependents?

## Step 3 — Identify Risks

Check for:
- Breaking changes
- Shared types that might need updates
- Database migrations needed
- Infrastructure changes required

## Step 4 — Infrastructure Safety Checks (if applicable)

If infrastructure changes are involved:

1. **Check existing AWS resources** for the target environment
2. **Verify Terraform S3 state backend** is accessible and working correctly
3. **Ensure no resources will be accidentally destroyed or duplicated**
4. **Apply Terraform patterns automatically** (see `.claude/rules/infrastructure/terraform-patterns.md`):
   - Favor official Terraform Registry modules over custom code
   - Use naming convention: `{project}-{environment}-{component}`
   - Apply tagging module for standard tags
   - Follow Flow A (ephemeral) for DB secrets or Flow B (external) for API keys
   - Implement least-privilege IAM policies

## Step 5 — Create Branch

```bash
git checkout develop && git pull
git checkout -b feature/<ticket-number>-<slug>
```

**Important:** Always branch from `develop`, not `master`. Ensure `develop` is up to date first.

## Step 6 — Produce Phased Plan

Create a phased plan with exact file paths and implementation steps. Wait for user confirmation before proceeding.

**Conventions applied automatically:**
- Branches created from `develop` (see `.claude/rules/common/git-workflow.md`)
- Commit messages use conventional commits format
- **Commits must be reversible** — small, focused commits organized by topic/domain
- Code comments follow style guidelines
- PR descriptions follow `.github/pull_request_template.md` when using `/raise-pr` and should be simple
- **Terraform patterns** (see `.claude/rules/infrastructure/terraform-patterns.md`)
