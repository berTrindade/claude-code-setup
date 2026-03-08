---
description: Full pre-PR verification — build, types, lint, tests, security scan.
argument-hint: []
allowed-tools: Bash
model: sonnet
---

Run these in order inside each affected package (`git diff --name-only origin/master...HEAD` to find them). Stop at the first failure and fix it before continuing.

```bash
npm run build
npx tsc --noEmit
npm run lint
npm test -- --coverage
npm audit --audit-level=high
```

Report: Build / Types / Lint / Tests / Security — PASS or FAIL for each. Overall: READY or NOT READY for PR.
