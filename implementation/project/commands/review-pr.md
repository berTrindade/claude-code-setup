---
description: Review a teammate's PR. Usage: /review-pr <number>
---

Review the pull request provided in the arguments (e.g. `/review-pr 42`).

## Step 1 — Read the PR and understand intention

```bash
gh pr view <number>
```

Understand:
- What is this PR trying to do? What's the main goal?
- What issue does it close?
- Are there any reviewer notes or prior comments?

**Check the linked ticket/issue:**
- Extract issue number from PR description
  - Look for patterns like "Closes #44", "Fixes #123", "Resolves #456", or just "#789"
  - Also check for full issue URLs
  - Extract just the number
- Fetch and read the linked issue/ticket:

```bash
gh issue view <issue-number>
```

**Understand scope and acceptance criteria:**
- Read the issue description to understand the problem being solved
- Review acceptance criteria (ACs) listed in the issue
- Understand the expected scope and boundaries of the work
- Note any constraints, requirements, or edge cases mentioned
- Check if the PR addresses all ACs or if some are deferred

**Look for hints about PR intention:**
- Check PR title and description for keywords like "first pass", "initial", "WIP", "draft", "proof of concept", "spike"
- Check PR labels (draft, WIP, first-pass, etc.)
- Look for comments mentioning follow-up PRs or iterative approach
- Understand if this is meant to be a complete solution or a first iteration
- Compare PR scope against issue ACs to see if this is a partial implementation

**If this appears to be a first pass:**
- Focus on whether it achieves the stated goal
- Only block on CRITICAL issues (security, breaking changes, data loss)
- Suggest follow-up PRs for non-critical improvements
- Be more lenient on polish, optimization, and best practices

## Step 2 — Get the code

First, fetch the latest changes from the remote:

```bash
git fetch origin
```

Then checkout the PR branch:

```bash
gh pr checkout <number> --force
```

Verify you're on the correct branch:

```bash
git branch --show-current
```

## Step 2.5 — Identify tools/services and fetch documentation

Before reviewing, identify which tools/services are used in the changed files and fetch their latest documentation:

```bash
git diff develop...HEAD --name-only
```

For each tool/service detected, fetch relevant documentation:

**If AWS/Terraform code changed:**
- Use `aws-docs` MCP server to fetch AWS service documentation
- Use `terraform` MCP server to fetch Terraform provider/resource docs
- Verify resource configurations against latest AWS best practices

**If frontend code changed:**
- Use `context7` MCP server to fetch framework documentation
- Check for API changes, deprecations, best practices

**If Express/Node API code changed:**
- Use `context7` MCP server to fetch Express documentation
- Verify middleware usage, routing patterns, etc.

**If database/PostgreSQL code changed:**
- Use `postgres` MCP server to query schema and verify patterns
- Check for N+1 queries, missing indexes, etc.

Document the key APIs/patterns you'll reference during the review.

## Step 3 — Review the diff

```bash
git diff develop...HEAD
```

Invoke the **code-reviewer** agent on the full diff. Read the changed files fully — not just the diff lines.

**Pass PR intention context to the reviewer:**
- Share what the PR is trying to achieve (from Step 1)
- Share the linked issue/ticket scope and acceptance criteria
- Indicate if this appears to be a first pass or iterative PR
- Note any hints found about follow-up PRs or iterative approach
- Compare PR implementation against issue ACs to identify if all requirements are met

**STRICT: Review ONLY PR commits, nothing else:**
- **ONLY** review files that appear in `git diff develop...HEAD` or `git diff --name-only`
- **DO NOT** explore unrelated parts of the codebase
- **DO NOT** read files that are not in the PR diff
- **ONLY** read additional files if absolutely necessary to understand a specific change

**Use the documentation fetched in Step 2.5** to verify:
- API usage matches latest documentation
- Patterns follow current best practices
- No deprecated APIs or methods are used
- Configuration matches recommended settings

## Step 4 — Output findings

**Format all comments using [Conventional Comments](https://conventionalcomments.org/) format.**

Group by file. Severity first: CRITICAL → HIGH → LOW.

### Comment Format

```
<label> [decorations]: <subject>

[discussion with citations/references]
```

**Labels to use:**
- `issue` - Problems that need fixing (use `(blocking)` decoration for CRITICAL/HIGH)
- `suggestion` - Proposed improvements (use `(blocking)` or `(non-blocking)` as appropriate)
- `nitpick` - Trivial preference-based requests (always `(non-blocking)`)
- `praise` - Positive feedback (always include at least one per review)
- `question` - Need clarification (use `(non-blocking)` if not blocking)
- `todo` - Small necessary changes
- `chore` - Process-related tasks
- `note` - Non-blocking informational comments

**Decorations:**
- `(blocking)` - Must be resolved before merge
- `(non-blocking)` - Can be addressed later
- `(if-minor)` - Only if changes are minor
- Domain-specific: `(security)`, `(test)`, `(ux)`, `(performance)`, `(api)`, etc.

### Writing Style

- **Tone:** Casual and friendly, like talking to a teammate
- **Length:** Maximum 2 lines for discussion text
- **Conciseness:** Get to the point quickly, no fluff
- **Punctuation:** Avoid em dashes and colons in discussion text

**Always include citations/references when possible.**

End with a verdict: **Approve / Request Changes / Comment**.

**Important:** Output the review findings to the session only. Do NOT post anything to GitHub or the PR.

## Step 5 — Return to your branch

```bash
git checkout -
```
