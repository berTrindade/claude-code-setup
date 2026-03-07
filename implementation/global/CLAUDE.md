# Personal Global Instructions

<!-- Project-specific context lives in each repo's .claude/CLAUDE.local.md -->

## Communication Style

- No emojis unless explicitly asked
- Concise responses — no filler phrases ("Great!", "Certainly!", "Of course!")
- Use markdown links `[file.ts](path)` for file references, never backticks
- When blocked, ask rather than brute-force retry

## Code Behaviour

- Never add docstrings, comments, or type annotations to code that wasn't changed
- No default exports in backend services — named exports only
- No `console.log` left in committed code
- Always confirm before pushing to remote or taking destructive git actions
- Prefer editing existing files over creating new ones

## Model Selection

- Haiku: one-off lookups, quick searches, simple summaries
- Sonnet (default): code writing, reviews, bug fixes, standard tasks
- Opus: architecture decisions, ADRs, system design, complex planning

## Thinking Mode & Output Styles

- Use `outputStyle: "explanatory"` for detailed output with ★ Insight boxes (configured in settings)
- Use `/model` to select model and reasoning level
- Use `/context` to see context usage
- Use `ultrathink` keyword for high-effort reasoning

## Agent Delegation

- Delegate when: task spans >3 files, involves a specialized domain (security, DB, infra), or can run in parallel
- Scope subagents tightly — limited tools = focused execution
- See `~/.claude/rules/agents.md` for full delegation guide

## Claude Code Concepts

### Commands, Agents, Skills

- **Commands:** Entry points in `.claude/commands/` or `~/.claude/commands/`
- **Agents:** Specialized subagents in `.claude/agents/` or `~/.claude/agents/`
- **Skills:** Reusable knowledge in `.claude/skills/<name>/SKILL.md`
- **Orchestration:** Command → Agent → Skill pattern for complex workflows

### Hooks

- Organized in `.claude/hooks/` directory structure
- PreToolUse, PostToolUse, and Stop hooks
- See `~/.claude/rules/hooks.md` for reference

### MCP Servers

- Global: `~/.claude/.mcp.json`
- Project: `.claude/.mcp.json`
- See project `.claude/rules/mcp-servers.md` for usage patterns

### Built-in Features

- **Checkpointing:** `/rewind` or Esc Esc to undo
- **Voice Mode:** `/voice` for hands-free interaction
- **Remote Control:** `/remote-control` for multi-device access
- **Git Worktrees:** Isolated branches for parallel development
- **Ralph Wiggum Loop:** Autonomous development plugin

### Advanced Features

- **Scheduled Tasks:** `/loop` for recurring monitoring (up to 3 days)
- **Agent Teams:** Parallel development with git worktrees and tmux
- **Cross-Model:** Use different models for planning vs implementation
- **Debugging:** Screenshots, background tasks, MCP for console logs

See project `.claude/rules/` for detailed documentation of all concepts.
