# Agent Delegation Guide

## Model Selection

| Model | Use When |
|-------|----------|
| Haiku | One-off lookups, grep searches, quick summaries, simple Q&A |
| Sonnet | Code writing, bug fixes, reviews, standard engineering tasks (default) |
| Opus | Architecture decisions, ADRs, system design, complex multi-step planning |

## When to Delegate to a Subagent

Delegate when **any** of these apply:

- Task spans more than 3 files
- Specialized domain: security, database, infrastructure, E2E testing
- Task can run in parallel with other work (e.g., code review while writing tests)
- Exploration would burn significant context from the main window
- Risk is high enough to warrant a dedicated reviewer

## Available Agents

| Agent | Trigger |
|-------|---------|
| `planner` | Planning a new feature or significant change |
| `architect` | System design, ADR creation, picking between approaches |
| `tdd-guide` | Writing any new feature or fixing a bug (write tests first) |
| `code-reviewer` | After writing or modifying code |
| `security-reviewer` | Code touches auth, user input, APIs, or sensitive data |
| `build-error-resolver` | Build or TypeScript errors blocking progress |
| `database-reviewer` | Writing SQL, migrations, schema changes, query tuning |
| `refactor-cleaner` | Removing dead code, consolidating duplicates |
| `e2e-runner` | Running or debugging Playwright E2E tests |

## Scoping Rules

- Give each agent only the tools it needs — no Write access for read-only agents
- Background agents for independent work; foreground when you need results before continuing
- One agent = one concern; don't bundle unrelated tasks into a single subagent
