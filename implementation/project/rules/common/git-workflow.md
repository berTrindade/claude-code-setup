# Git Workflow

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

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`

Examples:
```
feat: add lab result history endpoint
fix: correct MX record TTL in Route53
refactor: extract care plan action builder
```

## Commit Strategy: Reversible Commits

**All commits must be reversible** — this requires small, focused commits organized by topic/domain.

**Principles:**
- **Small commits** — Each commit should represent a single logical change
- **Topic/domain focused** — Group related changes together, separate unrelated changes
- **Easy to revert** — If a commit needs to be undone, it should affect only one concern
- **Clear boundaries** — Each commit should be independently reviewable and testable

## Pull Request Workflow

1. Title: `<type>: <short description>` (under 70 chars)
2. **PR descriptions must follow the template** at `.github/pull_request_template.md`
   - Keep descriptions **simple** and straightforward
3. Link the issue: `Closes #<number>`
4. Base branch for PRs: `develop` (unless hotfix, then `master`)

## Story Creation

When creating new stories/issues, **must follow the template** at `.github/ISSUE_TEMPLATE/story-template.md`
