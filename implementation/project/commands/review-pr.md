---
description: Review a teammate's PR. Usage: /review-pr <number>
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

First, fetch the latest changes from the remote:

```bash
git fetch origin
```

Then checkout the PR branch (this will pull the latest changes):

```bash
gh pr checkout <number> --force
```

The `--force` flag ensures that if the branch already exists locally, it will be reset to match the latest state of the PR.

Verify you're on the correct branch:

```bash
git branch --show-current
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

**Format all comments using [Conventional Comments](https://conventionalcomments.org/) format.**

Group by file. Severity first: CRITICAL → HIGH → LOW.

### Comment Format

Use the Conventional Comments format for all review comments:

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

### Example Comment Format

```
**issue (security,blocking):** Hardcoded API key detected

This exposes credentials in version control. Use environment variables or secrets management instead.

References:
- [OWASP: Secrets Management](https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_cryptographic_key)
- [AWS Secrets Manager Best Practices](https://docs.aws.amazon.com/secretsmanager/latest/userguide/best-practices.html)

File: `src/config.ts:42`
```

**Always include citations/references when possible:**
- Link to official documentation (Strapi, AWS, Terraform, React Native, etc.)
- Reference security best practices (OWASP, CWE, etc.)
- Cite performance guides (React performance, database optimization, etc.)
- Link to relevant GitHub issues, RFCs, or ADRs
- Reference style guides or coding standards

For each finding:

```
**<label> (<decorations>):** <subject>

<discussion with context and reasoning>

References:
- [Title](URL) - Brief description
- [Title](URL) - Brief description

File: `path:line`
```

End with a verdict: **Approve / Request Changes / Comment**.

**Important:** Output the review findings to the session only. Do NOT post anything to GitHub or the PR. The review is for the user's reference only.

## Step 5 — Return to your branch

```bash
git checkout -
```
