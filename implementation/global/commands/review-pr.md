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

## Step 3 — Review the diff

```bash
git diff master...HEAD
```

Invoke the **code-reviewer** agent on the full diff. Read the changed files fully — not just the diff lines.

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
