---
name: codemap-updater
description: Generate or update CODEMAP.md — a living architecture overview of the current project. Use after significant code changes, when onboarding, or whenever the codebase structure needs to be documented. Activate when the user asks to "update the codemap", "map the codebase", or "generate CODEMAP.md".
license: MIT
metadata:
  author: btrindadedeabreu
  version: 1.0.0
---

# Codemap Updater

Generates or refreshes `CODEMAP.md` at the repo root — a concise, always-current map of the codebase architecture.

## When to Use

- After adding a new package, module, or major feature
- When onboarding — understand the project quickly
- After a refactor that moves files or changes structure
- When `CODEMAP.md` is stale or missing

---

## Execution Steps

### Step 1 — Discover repo structure

```
Glob: **/package.json (not node_modules)
Read: root package.json (check workspaces field)
```

List all packages/apps. For each: name, path, one-line purpose from its `package.json` description field.

### Step 2 — Map entry points and key modules

For each package, identify:

- **Entry point** — `main`, `exports`, or `src/index.ts`
- **Key modules** — top-level directories under `src/`
- **External boundaries** — cross-package imports

Use `Glob` and `Grep` only. Do not read full file contents.

```
Glob: src/index.ts, src/main.ts, src/server.ts, src/app.ts

Grep: "import.*from.*packages/" — cross-package dependencies
Grep: "app.use\|router\|createServer" — API entry points
```

### Step 3 — Trace the main data flows

Find the primary user-facing flows (HTTP request → service → DB, event → handler → response). Draw as a plain-text diagram — no Mermaid unless the project already uses it.

### Step 4 — Write CODEMAP.md

Output format:

```markdown
# Codemap

> Last updated: <YYYY-MM-DD>

## Repo Structure

| Package | Path | Purpose |
|---------|------|---------|
| api | packages/api | REST API — Express v5, Cognito JWT auth |
| ... | ... | ... |

## Architecture

<plain-text diagram>

  platform-app (RN)
       │ HTTPS
  packages/api  ←→  packages/care-plan
       │
  PostgreSQL

## Critical Paths

### <Flow name>
1. Entry: `path/to/entry`
2. → `path/to/next`
3. → `path/to/db`

## Key Files

| File | Role |
|------|------|
| packages/api/src/index.ts | API server entry |
| ... | ... |
```

### Step 5 — Verify

- Cross-check package names against actual `package.json` names
- Verify all paths in Critical Paths and Key Files exist

---

## Rules

- Keep output under 120 lines — if longer, you're cataloguing not mapping
- No lists of every file — only architecturally significant ones
- Update the "Last updated" date every time
- Never describe implementation details — structure and flow only
- If `CODEMAP.md` already exists, diff against current structure and update only stale sections
- Skip `node_modules`, `dist`, `build`, `.git`, generated files
