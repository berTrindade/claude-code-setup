---
description: Full pre-PR verification — build, types, lint, tests, security scan.
---

Run in each affected package (`git diff --name-only origin/master...HEAD`). Stop at first failure.

```bash
npm run build
npx tsc --noEmit
npm run lint
npm test -- --coverage
npm audit --audit-level=high
```

If infrastructure changed:

```bash
cd infrastructure && terraform validate
```

Report PASS/FAIL per phase. Overall: READY or NOT READY for PR.
