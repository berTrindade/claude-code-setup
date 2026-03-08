---
description: Investigate and fix a bug. Starts with reproduction, not planning.
argument-hint: [issue-number]
allowed-tools: Read, Edit, Write, Bash, Grep, Glob
model: sonnet
---

A bug has been reported. Follow these steps in order.

## Step 1 — Understand the bug

Read the error, stack trace, or description provided. Ask for it if missing.

Identify:
- What is the expected behaviour?
- What is the actual behaviour?
- Which file or module is the likely entry point?

## Step 2 — Reproduce with a failing test

Before touching any code, write a test that captures the bug:

```bash
npm test   # or the relevant test command for this project
```

It must fail. If you cannot reproduce it with a test, stop and ask the user how they triggered it.

## Step 3 — Root cause

With the failing test in place, trace the code path:
- Read the stack trace top-to-bottom
- Follow the call chain from entry point to failure
- Identify the exact line where the wrong behaviour originates

State the root cause clearly before writing any fix.

## Step 4 — Fix

Apply the smallest possible change that fixes the root cause.

Rules:
- Touch only what is broken
- No refactoring of surrounding code
- No improvements beyond the bug

## Step 5 — Confirm

```bash
npm test
```

The previously failing test must now pass. All other tests must still pass. If any break, the fix is wrong — revisit Step 4.

## Step 6 — Branch and ship

```bash
git checkout -b fix/<issue-number>-<short-slug>
```

Then run `/verify` → `/raise-pr`.
