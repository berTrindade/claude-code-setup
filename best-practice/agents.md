# Agent Best Practices

Sub-agents are custom agents with their own name, color, tools, permissions, and model.

## Frontmatter Template

```yaml
---
name: agent-name
description: What the agent does and when to use it
tools: [Read, Grep, Glob, Bash]
model: sonnet
skills: [skill-name-1, skill-name-2]
mcpServers: [github, context7]
color: green
memory: project
background: false
isolation: worktree
---
```

## Field Descriptions

- **name** - Unique identifier for the agent
- **description** - Clear description of purpose and when to use
- **tools** - List of allowed tools
- **model** - Model to use (sonnet, opus, haiku)
- **skills** - Preloaded skills for domain knowledge
- **mcpServers** - MCP servers available to this agent
- **color** - Visual distinction (green, red, yellow, cyan, magenta)
- **memory** - Memory scope (user, project, local)
- **background** - Run in background (true/false)
- **isolation** - Git worktree isolation ("worktree")

## Example: code-reviewer

```yaml
---
name: code-reviewer
description: Reviews code for quality and security. Use after writing or modifying code.
tools: [Read, Grep, Glob, Bash]
model: sonnet
skills: []
mcpServers: [github]
color: green
memory: project
background: false
---
```

## Best Practices

- **Focused scope** - Each agent should have a clear, focused purpose
- **Preload skills** - Use skills field to preload domain knowledge
- **Visual distinction** - Use colors to distinguish agents in UI
- **Memory scope** - Choose appropriate memory scope (project for project-specific, user for global)
- **Tool restrictions** - Only grant necessary tools for security
