---
description: Write a new feature using Test-Driven Development. Enforces RED→GREEN→REFACTOR with 80%+ coverage.
argument-hint: [feature-description]
allowed-tools: Read, Edit, Write, Bash, Grep
model: sonnet
---

Write a new feature using Test-Driven Development (TDD) methodology.

## Step 1 — Delegate to TDD Guide Agent

Invoke the `tdd-guide` agent to enforce TDD practices.

## Step 2 — RED: Write Failing Test

Write a test that describes the desired behavior. It must fail first.

```bash
npm test
```

## Step 3 — GREEN: Make It Pass

Write the minimal code to make the test pass.

```bash
npm test
```

## Step 4 — REFACTOR: Improve Code

Refactor the code while keeping tests green. Ensure:
- Code is clean and maintainable
- Tests are well-structured
- Coverage is 80% or higher

## Step 5 — Verify Coverage

```bash
npm test -- --coverage
```

Ensure coverage meets the 80% threshold.

## Step 6 — Commit

```bash
git add .
git commit -m "test: <description>"
```

Follow conventional commits format.
