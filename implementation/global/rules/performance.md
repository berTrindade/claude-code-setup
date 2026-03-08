# Context Window & Performance

## Context Window Rules

- Target: keep active tools under 80, enabled MCPs under 10 per session
- Disable MCPs not needed for the current task (playwright when not testing, expo when not on mobile, posthog when not analysing events)
- Run `/compact` manually before starting a major new task in a long session
- Use subagents to protect the main context from large explorations — delegate codebase-wide searches

## MCP Hygiene

- Have MCPs installed globally but **disable per project** what isn't relevant
- Too many active tools = degraded performance and shorter effective context
- Check active tool count: `/plugins` → scroll to MCP section
- Rule of thumb: if you haven't used an MCP in the last 3 tasks, disable it for that project

## Model Cost vs. Capability

- Don't default to Opus for everything — Sonnet handles 90% of tasks well
- Use Haiku for subagents doing search/grep/file-read work to save cost and latency
- Reserve Opus for genuine architecture decisions where reasoning depth matters

## Long-Running Commands

- Dev servers, test watchers, and build processes must run in tmux (enforced by hook)
- Attach to running sessions: `tmux attach -t <session-name>`
- Don't queue up many messages while Claude is mid-task — wait for completion or interrupt with Esc Esc
