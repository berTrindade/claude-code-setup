# Performance Optimization

Tips for optimizing Claude Code workflows and performance.

## Context Management

- Keep context under control with `/context`
- Use `/clear` when switching tasks
- Keep CLAUDE.md under 200 lines
- Move detailed patterns to `.claude/rules/`

## Model Selection

- **Haiku** - Fast, simple tasks
- **Sonnet** - Balanced performance (most workflows)
- **Opus** - Complex reasoning

## Output Style Configuration

Configure output detail: explanatory (recommended), concise, or thinking.

## Tool Usage Optimization

- Use parallel tool calls when possible
- Batch related operations
- Use background tasks for long-running operations

## Workflow Optimization

- Single-purpose commands
- Reusable patterns
- Clear instructions
- Preload skills in agents
