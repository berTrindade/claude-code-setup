# Daily Workflows

## Occasion Map

| Situation | Command | What happens |
|-----------|---------|-------------|
| New ticket from the board | `/start-ticket <n>` | Fetches issue → spikes codebase → creates branch → produces plan → waits |
| Bug report (Slack, Sentry) | `/bug-fix` | Reproduce with failing test → root cause → minimal fix → verify |
| Production is down | `/hotfix` | Branch from master → smallest fix → fast PR, no full verify cycle |
| Reviewing a teammate's PR | `/review-pr <n>` | Reads PR + linked ticket → fetches latest code → auto-detects change types → invokes specialized agents → reviews with documentation → outputs Conventional Comments format → session-only (no GitHub posting) |
| Check if PR comments were addressed | `/check-pr-comments <n>` | Reviews PR again → checks if previous review comments were addressed → verifies fixes → reports on addressed/pending/new issues |
| Need to understand something | `/spike <question>` | Explores codebase + external sources (articles, GitHub, MCP docs) → compares approaches → evaluates for your context → structured findings |
| Before shipping | `/verify` → `/raise-pr` | All checks → lint/typecheck/tests/security → opens PR with full body |
| Create draft PR (WIP) | `/draft-pr` | Creates draft PR without verification — use for work-in-progress PRs |
| Architecture decision | `/plan` | Planner agent → phased plan → waits for your confirmation |
| Writing a new feature TDD | `/tdd` | Enforces RED→GREEN→REFACTOR, 80%+ coverage |
| Cleaning up dead code | `/refactor-clean` | knip/depcheck/ts-prune → safe removal, one batch at a time |
| Onboarding / after big refactor | `update the codemap` | Generates/refreshes `CODEMAP.md` — repo structure, data flows, key files |
| Running / debugging E2E tests | delegate to `e2e-runner` | Runs Playwright tests, interprets failures, suggests fixes — never touches app code |

---

## Workflows

### Start of Day

```bash
git checkout develop && git pull
```

Pick ticket → `/start-ticket <number>`

**Note:** All feature/fix branches are created from `develop`, not `master`.

---

### Feature / Fix Ticket — `/start-ticket 123`

1. Fetches GitHub issue + linked context
2. Spikes relevant code (`clinical → care-plan → curate → api → app`)
3. Identifies risks: breaking changes, shared types, DB migrations, infra
4. **If infrastructure changes are involved:**
   - Check existing AWS resources for the target environment
   - Verify Terraform S3 state backend is accessible and working correctly
   - Ensure no resources will be accidentally destroyed or duplicated
   - **Apply Terraform patterns automatically** (see `.claude/rules/infrastructure/terraform-patterns.md`):
     - Favor official Terraform Registry modules over custom code
     - Use naming convention: `{project}-{environment}-{component}`
     - Apply tagging module for standard tags
     - Use declarative API Gateway routes if adding endpoints
     - Follow Flow A (ephemeral) for DB secrets or Flow B (external) for API keys
     - Implement least-privilege IAM policies
     - Add VPC endpoints for private subnet access
     - Include observability (CloudWatch, X-Ray, Slack alerts)
5. Creates branch `feature/123-slug` or `fix/123-slug` **from `develop`** (ensures `develop` is up to date first)
6. Produces phased plan with exact file paths
7. **Waits for your go-ahead**

**Conventions applied automatically:**
- Branches created from `develop` (see `.claude/rules/common/git-workflow.md`)
- Commit messages use conventional commits format (see `.claude/rules/common/git-workflow.md`)
- **Commits must be reversible** — small, focused commits organized by topic/domain
- Code comments follow style guidelines (see `.claude/rules/code-style.md`)
- PR descriptions follow `.github/pull_request_template.md` when using `/raise-pr` and should be simple
- **Terraform patterns** (see `.claude/rules/infrastructure/terraform-patterns.md`):
  - Favor official Terraform Registry modules over custom code
  - Naming, tagging, module structure
  - Secrets management (Flow A/B)
  - API Gateway declarative routes
  - IAM least-privilege
  - Safety guardrails
- **Terraform style guide compliance** (see `.claude/rules/infrastructure/style-verification.md`):
  - Code formatting (`terraform fmt`)
  - Block ordering (count/for_each first, tags last, lifecycle last)
  - Naming conventions (lowercase with underscores)
  - Variable/output descriptions
  - Prefer `for_each` over `count` when appropriate

---

### Bug Fix — `/bug-fix`

1. Understand: expected vs actual, which package
2. Write a **failing test** that captures the bug — must fail first
3. Trace root cause through the call chain
4. Apply smallest possible fix — nothing else
5. Confirm the test now passes, all others still green
6. Branch → `/verify` → `/raise-pr`

---

### Hotfix (Production Down) — `/hotfix`

1. Stash current work, branch from `master`
2. Reproduce locally
3. Smallest fix only — no refactoring
4. Run only the affected package's tests (not full verify)
5. Open urgent PR: `hotfix: description`, base `master`
6. File follow-up ticket for proper fix if needed

---

### Reviewing a Teammate's PR — `/review-pr 42`

1. **Read PR and understand intention:**
   - Read PR description, title, labels
   - Extract and read linked issue/ticket (e.g., "Closes #44")
   - Understand scope and acceptance criteria (ACs)
   - Check for hints about "first pass", "WIP", "draft"
   - Compare PR implementation against issue ACs

2. **Get the code:**
   - `git fetch origin` - fetch latest changes
   - `gh pr checkout <number> --force` - checkout PR branch with latest changes
   - Verify branch with `git branch --show-current`

3. **Identify tools/services and fetch documentation:**
   - Analyze changed files: `git diff develop...HEAD --name-only`
   - Fetch relevant docs via MCP servers:
     - Strapi → `strapi-docs` MCP
     - AWS/Terraform → `aws-docs` + `terraform` MCPs
     - React Native/Expo → `context7` MCP
     - Express/Node → `context7` MCP
     - PostgreSQL → `postgres` MCP

4. **Review the diff:**
   - **STRICT SCOPE:** Only review files in `git diff develop...HEAD`
   - Auto-detect change types and invoke specialized agents:
     - Security changes → `security-reviewer` agent
     - Database changes → `database-reviewer` agent
     - Build errors → `build-error-resolver` agent
   - Reference relevant skills based on changes
   - Full diff review via `code-reviewer` agent

5. **Output findings:**
   - **Conventional Comments format** (https://conventionalcomments.org/)
   - **Kind and gentle tone** - casual, supportive, like helping a teammate
   - **Always include citations/references** when possible
   - Grouped by file, severity first: CRITICAL → HIGH → LOW
   - **If approving:** Include 1-line approval comment with PR number: `**praise:** Approved PR #[number]. [description]`
   - **Session-only output** - does NOT post to GitHub or PR

6. **Return to your branch:**
   - `git checkout -` - return to previous branch

**Key features:**
- Understands PR intention and handles first-pass PRs appropriately
- Only blocks on CRITICAL issues for first-pass PRs
- Suggests follow-up PRs for non-critical improvements
- Auto-detects change types and uses specialized agents
- Fetches latest documentation for accurate reviews
- Strict scope - only reviews PR commits, nothing else

---

### Checking if PR Comments Were Addressed — `/check-pr-comments 42`

Use this when you've reviewed a PR before and want to verify if your comments were addressed:

1. **Read PR and previous comments:**
   - Check PR comments and review threads
   - See what changed since your last review
   - Note which comments were addressed

2. **Get latest code:**
   - `git fetch origin` - fetch latest changes
   - `gh pr checkout <number> --force` - checkout PR branch
   - Review new commits since your last review

3. **Check specific comments:**
   - For each comment from your previous review
   - Verify if the issue was fixed
   - Check if fixes match your suggestions
   - Look for any new issues introduced

4. **Output findings:**
   - **Conventional Comments format**
   - Mark comments as addressed, not addressed, or partially addressed
   - Report any new issues found
   - Summary: All Addressed / Partially Addressed / Needs More Work

**Note:** This is session-only output, doesn't post to GitHub.

---

### Research Spike — `/spike how does care plan generation work` or `/spike should we use React Query`

Comprehensive research that explores both your codebase and external sources:

1. **States the question clearly** — breaks down vague questions into sub-questions
2. **Explores codebase** — traces data flow, identifies key files and dependencies
3. **Checks history** — git log and recent PRs for context
4. **Researches external sources:**
   - **Web search** for articles, best practices, and case studies
   - **GitHub search** for how others implement similar solutions
   - **MCP servers** for official docs (AWS, Terraform, Strapi, React Native/Expo, PostgreSQL)
5. **Compares approaches** — pros/cons, performance, maintenance, security
6. **Evaluates for your context** — aligns with your stack, patterns, constraints
7. **Outputs structured findings** with citations:
   - What exists in codebase
   - External research (articles, GitHub examples, official docs)
   - Evaluation of approaches
   - Recommendation with reasoning

**Use cases:**
- Understanding how something works in your codebase
- Evaluating an AI-suggested approach
- Comparing different solutions before implementing
- Researching best practices for a new feature

No code written. If implementation needed: `/start-ticket` or `/plan`.

---

### Creating a Draft PR — `/draft-pr`

Use for work-in-progress PRs that aren't ready for review yet:

1. Review changes: `git diff develop...HEAD`
2. Create draft PR: `gh pr create --base develop --draft --title "<type>: <description>" --body-file .github/pull_request_template.md`
3. Fill in PR template (test plan can be "TBD" for drafts)
4. Link to issue: `Closes #<issue-number>` or `Fixes #<issue-number>`
5. Mark as ready later: `gh pr ready <number>` when ready for review

**Note:** Draft PRs won't trigger review requests until marked as ready.

---

### Shipping — `/verify` → `/raise-pr`

`/verify` — build → typecheck → lint → tests → security scan → terraform validate (if infra changed)

**If infrastructure changed:**
- Check existing AWS resources for the target environment
- Verify Terraform S3 state backend is accessible and working correctly
- Review `terraform plan` to ensure no resources will be accidentally destroyed or duplicated
- **Verify Terraform patterns are followed:**
  - Official Terraform Registry modules used when available (avoid custom code)
  - Naming convention: `{project}-{environment}-{component}`
  - Tagging module applied to all resources
  - Secrets use Flow A (ephemeral) or Flow B (external) appropriately
  - IAM policies follow least-privilege
  - API Gateway uses declarative routes if applicable
- **Verify style guide compliance:**
  - Run `terraform fmt -recursive`
  - Run `terraform validate`
  - Check block ordering (count/for_each first, tags last, lifecycle last)
  - Verify naming conventions (lowercase with underscores)
  - Ensure all variables/outputs have descriptions
  - Prefer `for_each` over `count` when appropriate

`/raise-pr` — full checklist → lint/types/tests/review → opens PR following `.github/pull_request_template.md` with What/Why/How/Test plan

**Conventions applied automatically:**
- Commit messages use conventional commits (casual tone, no em dashes, no colons in description)
- **Commits must be reversible** — small, focused commits organized by topic/domain
- PR description follows the PR template structure and should be simple
- All commits follow `.claude/rules/common/git-workflow.md` guidelines
- **Terraform code follows patterns** (see `.claude/rules/infrastructure/terraform-patterns.md` and `secrets-management.md`)

---

## Agent Reference

| Agent | When Claude uses it |
|-------|-------------------|
| `planner` | `/plan`, `/start-ticket` |
| `architect` | Architecture decisions, ADR creation |
| `code-reviewer` | `/raise-pr`, `/review-pr` |
| `security-reviewer` | Auth/input/API endpoint changes |
| `tdd-guide` | `/tdd`, `/bug-fix` |
| `database-reviewer` | SQL, migrations, schema design |
| `build-error-resolver` | Build or TypeScript failures |
| `refactor-cleaner` | `/refactor-clean` |
| `e2e-runner` | Running/debugging Playwright E2E tests |

---

## Plugins

Managed via `/plugins`. Keep under 10 enabled — each adds tools that consume context window.

| Plugin | Source | What it provides | Enabled |
| ------ | ------ | ---------------- | ------- |
| `typescript-lsp` | `claude-plugins-official` | Real-time TypeScript type checking, go-to-definition, completions | Yes |
| `mgrep` | `Mixedbread-Grep` | Semantic + keyword search, better than ripgrep for fuzzy codebase queries | Yes |

> To add a marketplace: `claude plugin marketplace add <url>` then install via `/plugins`.

---

## Active Hooks (automatic)

Hooks are organized in `.claude/hooks/` directory structure. See `~/.claude/rules/hooks.md` for full reference.

| Event | Hook File | Trigger | Action |
| ----- | --------- | ------- | ------ |
| PreToolUse | `pre-tool-use/dev-server-block.md` | `npm run dev` / `pnpm dev` / etc. | Blocked — must use tmux |
| PreToolUse | `pre-tool-use/git-push-reminder.md` | `git push` | Reminder to review before pushing |
| PostToolUse | `post-tool-use/prettier-format.md` | `Edit`/`Write` any `.ts/.tsx/.js/.jsx/.json/.css` | Prettier auto-format |
| Stop | `stop/console-log-check.md` | End of session | Scans modified files for leftover `console.log` |

**Note:** Hooks have been migrated from `settings.local.json` to `.claude/hooks/` directory for better organization.

---

## MCP Servers

### Global (`~/.claude/.mcp.json`)

| Server | What it gives Claude | Auth |
|--------|---------------------|------|
| `github` | Read/write issues, PRs, repos | PAT in env |
| `sequential-thinking` | Structured multi-step reasoning | None |
| `context7` | Live docs for React Native, Expo, Express, Terraform | None |
| `aws-docs` | AWS service documentation | None (needs `uv`) |
| `aws` | Live AWS operations — query Cognito, S3, CloudFront, Route53 | Local AWS credentials |
| `playwright` | Browser automation for E2E debugging | None |
| `obsidian` | Personal notes vault | None |

### platform (`.claude/.mcp.json`)

| Server | What it gives Claude | Auth |
|--------|---------------------|------|
| `postgres` | Query the database directly (`@zeddotdev/postgres-context-server` — secure fork) | Connection string in args |
| `posthog` | Analytics, feature flags, SQL insights | Personal API key in env |
| `terraform` | Terraform Registry — provider/resource/module docs | None (needs Docker) |
| `expo` | Live Expo dev server — bundle info, logs, device state | None (dev server on :8081) |
| `playwright` | Run and debug platform E2E tests | None |

### Which MCPs Claude uses per workflow

| Workflow | MCPs |
|----------|------|
| `/start-ticket` | `github` · `postgres` (schema check) · `context7` (framework docs) |
| `/bug-fix` | `posthog` (error scope) · `postgres` (data state) · `context7` · `playwright` (E2E repro) |
| `/hotfix` | `posthog` (blast radius) · `postgres` (data check) · `aws` (live infra state) |
| `/review-pr` | `github` · `strapi-docs` · `aws-docs` · `terraform` · `context7` · `postgres` (auto-detects change types and fetches relevant docs) |
| `/spike` | `postgres` · `posthog` (usage patterns) · `sequential-thinking` · `aws-docs` · `terraform` · `strapi-docs` · `context7` · `github` (code search) · `web-search` |
| `/plan` | `sequential-thinking` · `github` |
| `/raise-pr` | `github` |
| Mobile (Expo / RN) | `expo` (dev server) · `playwright` (E2E) · `context7` (RN/Expo docs) |
| Terraform / AWS infra | `terraform` (Registry schemas) · `aws-docs` (service docs) · `aws` (live state) |

### Setup checklist

See `.claude/rules/mcp-servers.md` for comprehensive MCP server documentation.

#### Credentials

- [ ] **URGENT** — Rotate GitHub PAT (exposed in conversation) → [github.com/settings/tokens](https://github.com/settings/tokens) → replace in `~/.claude/.mcp.json`
- [ ] Replace `YOUR_DATABASE_URL_HERE` in `.claude/.mcp.json` → local dev DB URL
- [ ] AWS credentials configured locally: `aws configure` (used by `aws` MCP)
- [ ] Set `POSTHOG_PERSONAL_API_KEY` environment variable for PostHog MCP

#### Runtime (not persistent)

- [ ] Expo dev server on `:8081` must be running for `expo` MCP — `cd platform-app && npx expo start`
- [ ] Docker running for `terraform` MCP server
- [ ] `uv` installed for `aws-docs` MCP server

---

## Built-in Features

### Checkpointing

- **Rewind:** Press `Esc Esc` or use `/rewind` command
- **When to use:** When Claude goes off-track or makes unwanted changes
- **Alternative:** Use `/clear` to reset context mid-session

See `.claude/rules/built-in-features.md` for details.

### Voice Mode

- **Activate:** `/voice` command
- **Use cases:** Hands-free interaction, verbal descriptions

### Remote Control

- **Activate:** `/remote-control` command
- **Use cases:** Continue sessions from mobile/tablet, headless mode

### Git Worktrees

- **Usage:** Isolated branches for parallel development
- **Benefits:** Each agent gets its own working copy
- **Best practice:** Use with agent teams and tmux

See `.claude/rules/built-in-features.md` for complete documentation.

---

## Advanced Features

### Scheduled Tasks (`/loop`)

Run prompts on a recurring schedule (up to 3 days):
- Poll deployments: `/loop 5m check if deployment is complete`
- Monitor PRs: `/loop 10m check if PR has new comments`
- One-time reminders: `/loop 1h remind me to review the PR`

### Agent Teams

Multiple agents working in parallel:
- Use git worktrees for isolation
- Use tmux for session management
- Set `CLAUDE_TEAM_ID` environment variable for coordination

### Thinking Mode & Output Styles

- Configure in `settings.json`: `alwaysThinkingEnabled`, `outputStyle`
- Use `/model` to select model and reasoning level
- Use `/context` to see context usage
- Use `ultrathink` keyword for high-effort reasoning

### Cross-Model Workflow

Use different models for different stages:
- Opus for planning and architecture
- Sonnet for implementation
- Opus for comprehensive review

### Debugging Practices

- **Screenshots:** Share visual context with Claude
- **Background tasks:** Run terminal commands in background for logs
- **MCP for console logs:** Use Chrome/Playwright MCPs for debugging
- **ASCII diagrams:** Explain architecture visually
- **Troubleshooting:** Use `/doctor` for diagnostics

See `.claude/rules/advanced-features.md` for complete documentation.

---

## CLI & Utilities

### Session Management

- `/rename` - Rename current session
- `/resume` - Continue previous session
- `/rewind` (Esc Esc) - Undo changes
- `/compact` - Reduce context usage
- `/clear` - Reset context
- `/fork` - Branch conversation

### Status Line

Customizable status bar showing:
- Context usage
- Current model
- Cost tracking
- Session info

Configure in `settings.json`:
```json
{
  "statusline": {
    "enabled": true,
    "showContext": true,
    "showModel": true,
    "showCost": true
  }
}
```

### Usage Monitoring

- `/usage` - Check plan limits
- `/extra-usage` - Configure overflow billing
- `/cost` - View token usage

See `.claude/rules/cli-reference.md` for complete CLI documentation.

---

## Orchestration Pattern

Commands → Agents → Skills workflow pattern:

1. **Command** - Entry point, user interaction (`.claude/commands/`)
2. **Agent** - Fetches data with preloaded skill (`.claude/agents/` or `~/.claude/agents/`)
3. **Skill** - Creates output independently (`.claude/skills/<name>/SKILL.md`)

See `.claude/rules/workflows.md` for detailed orchestration patterns.
