# Git Workflow

## Branch Strategy

- **Always create branches from `develop`** — ensure `develop` is up to date before branching
- Base branch for all feature/fix/refactor branches: `develop`
- Only hotfixes branch from `main` (production)

## Branch Naming

- `feature/<issue-number>-<short-slug>` — new functionality
- `fix/<issue-number>-<short-slug>` — bug fix
- `refactor/<issue-number>-<short-slug>` — refactoring, no behavior change

## Commit Message Format

Use **conventional commits** format:

```
<type>: <description>

<optional body>
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
chore: update Terraform provider versions
```

## Commit Strategy: Reversible Commits

**All commits must be reversible** — this requires small, focused commits organized by topic/domain.

**Principles:**
- **Small commits** — Each commit should represent a single logical change
- **Topic/domain focused** — Group related changes together, separate unrelated changes
- **Easy to revert** — If a commit needs to be undone, it should affect only one concern
- **Clear boundaries** — Each commit should be independently reviewable and testable

**Guidelines:**
- One feature/change per commit (not multiple unrelated changes)
- Separate commits for different domains (e.g., API changes vs UI changes vs infrastructure)
- Separate commits for different concerns (e.g., feature implementation vs tests vs refactoring)
- If a commit touches multiple files, ensure they're all part of the same logical change
- Prefer multiple small commits over one large commit

**Good Examples:**
```
feat: add lab result history endpoint
test: add tests for lab result history endpoint
docs: update API docs for lab result history

fix: correct MX record TTL in Route53
chore: update Terraform provider versions
```

**Bad Examples:**
```
feat: add lab result history and fix MX records and update docs
# ❌ Multiple unrelated changes in one commit

feat: implement lab result feature
# ❌ Too large - includes API, tests, UI, and docs all together
```

**When to combine commits:**
- Only when changes are tightly coupled and cannot be meaningfully separated
- When reverting one would require reverting the other
- When they represent a single atomic operation (e.g., adding a file and its tests together)

## Pull Request Workflow

1. Analyze the **full commit history** (`git diff develop...HEAD`), not just the latest commit
2. Title: `<type>: <short description>` (under 70 chars)
3. **PR descriptions must follow the template** at `.github/pull_request_template.md`
   - Keep descriptions **simple** and straightforward
   - Include ticket number, description, platform checkboxes, screenshots (if applicable), deployment info, and checklist
4. Link the issue: `Closes #<number>`
5. Base branch for PRs: `develop` (unless hotfix, then `main`)
6. Run `/raise-pr` skill to verify all checks pass before opening

## Story Creation

When creating new stories/issues, use `/create-story` command or **must follow the template** at `.github/ISSUE_TEMPLATE/story-template.md`:
- Include summary with "As a... I want... So that..." format
- Add acceptance criteria with Given/When/Then structure
- Include analytics events following `.claude/rules/product/analytics-naming.md`
- List dependencies and related PRs
- Complete the Definition of Done checklist

## Before Committing

- Run `npm run lint:fix && npm run format` in affected packages
- No `console.log` left in
- No hardcoded secrets
- Tests pass

## Before Merging

- CI checks green (lint, typecheck, build, Trivy)
- Code review approved
- **If infrastructure changed:**
  - Check existing AWS resources for the target environment
  - Verify Terraform S3 state backend is accessible and working correctly
  - Review `terraform plan` to ensure no resources will be accidentally destroyed or duplicated
