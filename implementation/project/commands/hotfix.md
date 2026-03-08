---
description: Fast path for production issues. Branch from master, minimal fix, urgent PR.
---

Production is broken. Move fast but don't skip safety.

## Step 1 — Assess

Confirm this is actually a production issue before proceeding:
- Is it affecting users right now?
- Is there a workaround?

If it can wait until the next normal release, use `/bug-fix` instead.

## Step 2 — Branch from master

```bash
git stash  # stash any current work
git checkout master && git pull
git checkout -b hotfix/<short-slug>
```

Never branch from a feature branch.

## Step 3 — Reproduce

Write the smallest possible failing test or confirm you can reproduce the issue locally.

## Step 4 — Fix

Smallest possible change. No refactoring. No improvements. One thing only.

## Step 5 — Test the affected package only

```bash
# Run only the package that changed — skip the full verify cycle
npm test
npm run lint
```

## Step 6 — Open urgent PR

```bash
gh pr create \
  --base master \
  --title "hotfix: <description>" \
  --body "## What broke
<description>

## Fix
<what changed and why>

## Tested
<how you confirmed the fix works>

## Follow-up
<any cleanup or proper fix needed after this ships>"
```

## Step 7 — Note follow-up work

If the hotfix is a patch rather than a proper fix, create a follow-up ticket immediately so it doesn't get forgotten.
