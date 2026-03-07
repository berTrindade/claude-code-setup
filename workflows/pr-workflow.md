# PR Workflow

How to create and review pull requests.

## Creating a PR

### Step 1: Verify Everything

Run `/verify` first to ensure:
- Build passes
- Type checking passes
- Linting passes
- Tests pass with coverage
- Security scan passes
- Terraform validate (if infrastructure changed)

### Step 2: Review Changes

```bash
git diff develop...HEAD
```

Review all changes to ensure they align with the ticket requirements.

### Step 3: Create PR

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

**Important:** Keep the PR description **simple** and focused.

## PR Description Requirements

- **Must follow template** at `.github/pull_request_template.md`
- Keep descriptions **simple** and straightforward
- Include ticket number, description, platform checkboxes, screenshots (if applicable), deployment info, and checklist

## Reviewing a PR

Use `/review-pr <number>`:

1. Reads PR description + linked issue
2. Checks out branch locally
3. Full diff review via `code-reviewer` agent
4. Findings grouped by file: CRITICAL → HIGH → LOW
5. Optionally posts `gh pr review` comment to GitHub
6. Returns you to your previous branch

## Conventions

- Commit messages use conventional commits (casual tone, no em dashes, no colons in description)
- **Commits must be reversible** — small, focused commits organized by topic/domain
- PR description follows the PR template structure and should be simple
- All commits follow `.claude/rules/common/git-workflow.md` guidelines
