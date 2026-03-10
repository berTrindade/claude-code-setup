---
description: Check if comments from a previous PR review were addressed. Usage: /check-pr-comments <number>
argument-hint: [pr-number]
allowed-tools: Read, Bash, Grep, Glob
model: sonnet
---

Check if comments from a previous PR review were addressed. This is useful when you've reviewed a PR before and want to verify if the author addressed your feedback.

## Step 1 — Read the PR and previous comments

```bash
gh pr view <number>
```

Understand:
- What changes were made since your last review?
- Are there new commits addressing your comments?
- What's the current state of the PR?

**Check for your previous review comments:**
- Look at PR comments and review threads
- Check if there are responses to your previous feedback
- Note which comments were addressed and which are still pending

## Step 2 — Get the latest code

First, fetch the latest changes from the remote:

```bash
git fetch origin
```

Then checkout the PR branch (this will pull the latest changes):

```bash
gh pr checkout <number> --force
```

Verify you're on the correct branch:

```bash
git branch --show-current
```

## Step 3 — Review what changed since your last review

```bash
git log --oneline HEAD~<number-of-commits-since-review>..HEAD
```

Or if you know the commit hash from your last review:

```bash
git diff <last-review-commit>...HEAD
```

**Focus on:**
- Changes that address your previous comments
- New commits that might have introduced new issues
- Whether the fixes match your suggestions

## Step 4 — Check specific comments

For each comment you made in your previous review:

1. **Identify the file and line** from your previous review notes
2. **Check if the issue was fixed:**
   ```bash
   git diff develop...HEAD -- <file-path>
   ```
3. **Verify the fix:**
   - Does it address the concern?
   - Is it implemented correctly?
   - Are there any new issues introduced?

## Step 5 — Output findings

**Format findings using [Conventional Comments](https://conventionalcomments.org/) format:**

For each previous comment:

**If addressed:**
```
**praise:** Fixed the N+1 query issue

Nice work batching these queries. This looks good now.

File: `src/services/users.ts:156`
```

**If not addressed:**
```
**issue (performance,blocking):** N+1 query still present

This still hits the DB for each item. We discussed batching this earlier.

File: `src/services/users.ts:156`
```

**If partially addressed:**
```
**suggestion (performance,non-blocking):** N+1 query improved but could be better

Good progress batching some queries, but this loop still runs a query per iteration. Maybe we can optimize this further.

File: `src/services/users.ts:156`
```

**If new issues found:**
```
**issue (security,blocking):** New hardcoded API key detected

Hey, just noticed this will expose credentials in git. Maybe we can move it to env vars.

References:
- [OWASP Use of Hard-coded Cryptographic Key](https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_cryptographic_key)

File: `src/config.ts:42`
```

**Be kind and gentle - make comments sound casual and supportive:**
- Very casual, kind, and gentle - like helping a teammate
- Frame feedback as helpful suggestions, not demands
- Use kind language: "Maybe we could...", "What do you think about...", "I'd suggest..."
- Acknowledge good work when comments are addressed
- Maximum 2 lines for discussion text
- Always include citations/references when possible

## Step 6 — Summary

End with:
1. Summary of comments addressed vs pending
2. Any new issues found
3. Verdict: **All Addressed / Partially Addressed / Needs More Work**

**Important:** Output the review findings to the session only. Do NOT post anything to GitHub or the PR. The review is for your reference only.

## Step 7 — Return to your branch

```bash
git checkout -
```
