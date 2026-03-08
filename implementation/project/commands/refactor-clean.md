---
description: Clean up dead code safely. Uses knip/depcheck/ts-prune to find unused code, removes one batch at a time.
argument-hint: []
allowed-tools: Read, Edit, Write, Bash, Grep
model: sonnet
---

Clean up dead code safely using automated tools.

## Step 1 — Delegate to Refactor Cleaner Agent

Invoke the `refactor-cleaner` agent to:
- Analyze codebase for dead code
- Identify unused exports, imports, and dependencies
- Suggest safe removals

## Step 2 — Run Analysis Tools

```bash
# Find unused exports
npx knip

# Find unused dependencies
npx depcheck

# Find unused TypeScript code
npx ts-prune
```

## Step 3 — Review Findings

Review the analysis results:
- Verify code is truly unused
- Check for false positives
- Identify dependencies that might be needed at runtime

## Step 4 — Remove One Batch

Remove one category of dead code at a time:
1. Unused imports
2. Unused exports
3. Unused dependencies
4. Unused files

## Step 5 — Verify After Each Batch

```bash
npm run build
npm test
npm run lint
```

Ensure nothing breaks after each removal.

## Step 6 — Commit

```bash
git add .
git commit -m "refactor: remove unused <category>"
```

Follow conventional commits format.
