---
description: Create a pull request following the PR template. Runs full verification first.
argument-hint: []
allowed-tools: Read, Edit, Write, Bash, Grep
model: sonnet
---

Create a pull request following the project's PR template.

## Step 1 — Verify Everything

Run `/verify` first to ensure:
- Build passes
- Type checking passes
- Linting passes
- Tests pass with coverage
- Security scan passes
- Terraform validate (if infrastructure changed)

## Step 2 — Review Changes

```bash
git diff develop...HEAD
```

Review all changes to ensure they align with the ticket requirements.

## Step 3 — Create PR

```bash
gh pr create \
  --base develop \
  --title "<type>: <description>" \
  --body-file .github/pull_request_template.md
```

Fill in the PR template:
- **What:** What does this PR do?
- **Why:** Why is this change needed?
- **How:** How was it implemented?
- **Test Plan:** How was it tested?

**Important:** Keep the PR description **simple** and focused. **Avoid em dashes** in PR descriptions.

## Step 4 — Apply Conventions

Ensure:
- Commit messages use conventional commits (casual tone, no em dashes, no colons in description)
- **PR descriptions must not use em dashes**
- **Commits must be reversible** — small, focused commits organized by topic/domain
- PR description follows the PR template structure and should be simple
- All commits follow `.claude/rules/common/git-workflow.md` guidelines
- **Terraform code follows patterns** (see `.claude/rules/infrastructure/terraform-patterns.md`)
