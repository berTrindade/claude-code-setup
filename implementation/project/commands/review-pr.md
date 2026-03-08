---
description: Review a teammate's PR. Usage: /review-pr <number>
argument-hint: [pr-number]
allowed-tools: Read, Bash, Grep, Glob
model: sonnet
---

Review the pull request provided in the arguments (e.g. `/review-pr 42`).

## Step 1 — Read the PR

```bash
gh pr view <number>
```

Understand:
- What is this PR trying to do?
- What issue does it close?
- Are there any reviewer notes or prior comments?

## Step 2 — Get the code

```bash
gh pr checkout <number>
```

## Step 2.5 — Identify tools/services and fetch documentation

Before reviewing, identify which tools/services are used in the changed files and fetch their latest documentation:

```bash
git diff master...HEAD --name-only
```

For each tool/service detected, fetch relevant documentation:

**If Strapi code changed:**
- Use `strapi-docs` MCP server to fetch latest Strapi API documentation
- Check for plugin APIs, content types, lifecycle hooks, etc.

**If AWS/Terraform code changed:**
- Use `aws-docs` MCP server to fetch AWS service documentation
- Use `terraform` MCP server to fetch Terraform provider/resource docs
- Verify resource configurations against latest AWS best practices

**If React Native/Expo code changed:**
- Use `context7` MCP server to fetch React Native/Expo documentation
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
git diff master...HEAD
```

Invoke the **code-reviewer** agent on the full diff. Read the changed files fully — not just the diff lines.

**Use the documentation fetched in Step 2.5** to verify:
- API usage matches latest documentation
- Patterns follow current best practices
- No deprecated APIs or methods are used
- Configuration matches recommended settings

## Step 4 — Output findings

Group by file. Severity first: CRITICAL → HIGH → LOW.

For each issue:
```
[CRITICAL|HIGH|LOW] Title
File: path:line
Issue: what's wrong
Suggestion: how to fix it
```

End with a verdict: **Approve / Request Changes / Comment**.

**Important:** Output the review findings to the session only. Do NOT post anything to GitHub or the PR. The review is for the user's reference only.

## Step 5 — Return to your branch

```bash
git checkout -
```
