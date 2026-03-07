# Claude Code Setup

My personal Claude Code configuration and workflows, shared for team collaboration and best practices.

**Purpose:** Document and share Claude Code setup, workflows, and best practices with company and peers.

## Quick Start

1. **Global Setup:** See [setup/global-setup.md](setup/global-setup.md)
2. **Project Setup:** See [setup/project-setup.md](setup/project-setup.md)
3. **Best Practices:** See [best-practice/](best-practice/) directory
4. **Implementation Examples:** See [implementation/](implementation/) directory

## Concepts

| Feature | Location | Description |
|---------|----------|-------------|
| **Commands** | `.claude/commands/<name>.md` | Entry-point prompts for workflows — invoke with `/command-name` |
| **Sub-Agents** | `.claude/agents/<name>.md` | Custom agents with tools, permissions, and model |
| **Skills** | `.claude/skills/<name>/SKILL.md` | Reusable knowledge, workflows, and slash commands |
| **Workflows** | `.claude/WORKFLOWS.md` | Daily workflow patterns |
| **Hooks** | `.claude/hooks/` | Deterministic scripts on specific events |
| **MCP Servers** | `.claude/.mcp.json` | Model Context Protocol connections |
| **Plugins** | Managed via `/plugins` | Bundles of skills, agents, hooks, MCPs |
| **Settings** | `.claude/settings.json` | Hierarchical configuration |
| **Status Line** | `.claude/settings.json` | Customizable status bar |
| **Memory** | `CLAUDE.md`, `.claude/rules/` | Persistent context |
| **Checkpointing** | Automatic (git-based) | Rewind with Esc Esc or `/rewind` |
| **CLI Flags** | Command-line | Startup flags and subcommands |

## Hot Features

- **Scheduled Tasks (`/loop`)** - Recurring monitoring up to 3 days
- **Agent Teams** - Parallel development with git worktrees
- **Voice Mode** - `/voice` for hands-free interaction
- **Remote Control** - Continue sessions from any device
- **Git Worktrees** - Isolated branches for parallel work
- **Ralph Wiggum Loop** - Autonomous development plugin

## Repository Structure

```
claude-code-setup/
├── README.md                    # This file
├── LICENSE                      # MIT License
├── .gitignore                  # Exclude sensitive files
├── setup/                      # Setup instructions
│   ├── global-setup.md        # How to set up ~/.claude/
│   ├── project-setup.md       # How to set up .claude/ in projects
│   └── prerequisites.md        # Required tools and versions
├── best-practice/              # Best practices documentation
│   ├── commands.md            # Command patterns
│   ├── agents.md              # Agent configuration
│   ├── workflows.md           # Workflow patterns
│   ├── hooks.md               # Hook patterns
│   ├── mcp-servers.md         # MCP configuration
│   ├── terraform-patterns.md  # Terraform-specific patterns
│   └── git-workflow.md        # Git workflow conventions
├── implementation/             # Actual implementation examples
│   ├── global/                # ~/.claude/ structure (sanitized)
│   └── project/               # .claude/ structure (sanitized)
├── workflows/                  # Workflow documentation
│   ├── ticket-workflow.md     # Ticket workflow
│   ├── infrastructure-workflow.md # Infrastructure workflow
│   └── pr-workflow.md         # PR workflow
├── tips/                       # Tips and tricks
│   ├── debugging.md           # Debugging practices
│   └── troubleshooting.md     # Common issues
└── changelog/                  # Change history
    └── CHANGELOG.md            # Track improvements
```

## Contributing

This repository documents my personal Claude Code setup. Feel free to:
- Fork and adapt for your own use
- Suggest improvements via issues
- Share your own patterns and workflows

## License

MIT License - See [LICENSE](LICENSE) file
