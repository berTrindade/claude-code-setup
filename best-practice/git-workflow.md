# Git Workflow Best Practices

## Branch Strategy

- **Always create branches from `develop`** — ensure `develop` is up to date before branching
- Base branch for all feature/fix/refactor branches: `develop`
- Only hotfixes branch from `master` (production)

## Commit Message Format

Use **conventional commits** format:

```
<type>: <description>
```

**Style Guidelines:**
- Use casual tone in descriptions
- Avoid em dashes in commit messages
- Avoid colons in the description text (the separator colon after type is required)
- Keep descriptions concise and clear

**Types:** `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`

**Examples:**
```
feat: add lab result history endpoint
fix: correct MX record TTL in Route53
refactor: extract care plan action builder
```

## Reversible Commits

**All commits must be reversible** — small, focused commits organized by topic/domain.

**Principles:**
- **Small commits** — Each commit represents a single logical change
- **Topic/domain focused** — Group related changes together
- **Easy to revert** — If a commit needs to be undone, it should affect only one concern

**Good Examples:**
```
feat: add lab result history endpoint
test: add tests for lab result history endpoint
docs: update API docs for lab result history
```

**Bad Examples:**
```
feat: add lab result history and fix MX records and update docs
# ❌ Multiple unrelated changes in one commit
```

## PR Descriptions

- **Must follow template** at `.github/pull_request_template.md`
- Keep descriptions **simple** and straightforward
- Include ticket number, description, platform checkboxes, screenshots (if applicable), deployment info, and checklist

## Story Creation

- **Must follow template** at `.github/ISSUE_TEMPLATE/story-template.md`
- Include summary with "I want to... So that..." format
- Add acceptance criteria with Given/When/Then structure
