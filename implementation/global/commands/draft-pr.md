---
description: Create a draft pull request following the PR template. Use for work-in-progress PRs that aren't ready for review yet.
argument-hint: []
allowed-tools: Read, Edit, Write, Bash, Grep
model: sonnet
---

Create a draft pull request following the project's PR template. Draft PRs are marked as "Draft" on GitHub and won't trigger review requests until marked as ready.

## Step 1 — Review Changes

```bash
git diff develop...HEAD
```

Review all changes to ensure they align with the ticket requirements.

## Step 2 — Create Draft PR

```bash
gh pr create \
  --base develop \
  --draft \
  --title "<type>: <description>" \
  --body-file .github/pull_request_template.md
```

Fill in the PR template:
- **What:** What does this PR do?
- **Why:** Why is this change needed?
- **How:** How was it implemented?
- **Test Plan:** How was it tested? (can be "TBD" for draft PRs)

**Important:** Keep the PR description **simple** and focused.

## Step 3 — Apply Conventions

Ensure:
- Commit messages use conventional commits (casual tone, no em dashes, no colons in description)
- **Commits must be reversible** — small, focused commits organized by topic/domain
- PR description follows the PR template structure and should be simple
- All commits follow `.claude/rules/common/git-workflow.md` guidelines

## Step 4 — Link to Issue

Make sure the PR description includes:
- `Closes #<issue-number>` or `Fixes #<issue-number>` to link to the ticket

**Note:** Draft PRs can be updated and marked as ready for review later using `gh pr ready <number>`.
