# Git Workflow Rules

## Branching Strategy

- **`main`** - Production-ready code (protected)
- **`develop`** - Integration branch for features
- **`feature/<ticket-number>-<description>`** - Feature branches (from `develop`)
- **`fix/<ticket-number>-<description>`** - Bug fixes (from `develop`)
- **`hotfix/<description>`** - Critical production fixes (from `main`)

## Commit Conventions

**Format:** `<type>: <description>`

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code style changes (formatting, no logic change)
- `refactor` - Code refactoring
- `test` - Adding or updating tests
- `chore` - Maintenance tasks

**Guidelines:**
- No em dashes in commit messages
- Casual, conversational tone
- No colon after type (e.g., `feat: add user auth`, not `feat: Add user auth`)
- Keep commits small and reversible
- One commit per topic/domain
- Comments should be one line when needed

## PR Descriptions

- Keep PR descriptions simple and clear
- Reference ticket numbers
- Include testing instructions if applicable
- Follow PR template if available

## Branch Creation

Always create branches from the latest `develop` branch:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/<ticket-number>-<description>
```

Exception: Hotfixes branch from `main`.
