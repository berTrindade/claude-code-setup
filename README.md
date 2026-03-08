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
| **Commands** | `.claude/commands/<name>.md` | Entry-point prompts for workflows |
| **Sub-Agents** | `.claude/agents/<name>.md` | Custom agents with tools, permissions, and model |
| **Skills** | `.claude/skills/<name>/SKILL.md` | Reusable knowledge and workflows |
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
├── README.md                    # Main documentation with concepts table
├── LICENSE                      # MIT or similar
├── .gitignore                  # Exclude sensitive files
├── setup/                      # Setup instructions
│   ├── global-setup.md        # How to set up ~/.claude/
│   ├── project-setup.md       # How to set up .claude/ in projects
│   └── prerequisites.md        # Required tools and versions
├── best-practice/              # Best practices documentation
│   ├── commands.md            # Command patterns and examples
│   ├── agents.md              # Agent configuration patterns
│   ├── skills.md              # Skill structure and usage
│   ├── workflows.md           # Workflow patterns
│   ├── hooks.md               # Hook examples and patterns
│   ├── mcp-servers.md         # MCP server configuration
│   ├── terraform-patterns.md  # Terraform-specific patterns
│   └── git-workflow.md        # Git workflow conventions
├── implementation/             # Actual implementation examples
│   ├── global/                # ~/.claude/ structure (sanitized)
│   │   ├── commands/         # Example commands
│   │   ├── agents/            # Example agents
│   │   ├── rules/             # Example rules
│   │   └── settings.json      # Example settings (no secrets)
│   └── project/               # .claude/ structure (sanitized)
│       ├── commands/          # Project commands
│       ├── agents/            # Project agents
│       ├── rules/              # Project rules
│       ├── skills/            # Project skills
│       ├── hooks/             # Project hooks
│       └── settings.local.json # Example project settings
├── workflows/                  # Workflow documentation
│   ├── ticket-workflow.md     # How to work on tickets
│   ├── infrastructure-workflow.md # Infrastructure changes workflow
│   └── pr-workflow.md         # PR creation and review workflow
├── tips/                       # Tips and tricks
│   ├── debugging.md           # Debugging practices
│   ├── performance.md         # Performance optimization
│   └── troubleshooting.md     # Common issues and solutions
└── changelog/                  # Change history
    └── setup-changes.md        # Track setup improvements
```

## Contributing

This repository documents my personal Claude Code setup. Feel free to:
- Fork and adapt for your own use
- Suggest improvements via issues
- Share your own patterns and workflows

## License

MIT License - See [LICENSE](LICENSE) file
