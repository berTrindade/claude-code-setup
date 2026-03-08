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
├── LICENSE                      # MIT License
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
├── implementation/             # Actual implementation examples (sanitized)
│   ├── global/                # ~/.claude/ structure
│   │   ├── commands/         # Global commands (5 files)
│   │   │   ├── bug-fix.md
│   │   │   ├── hotfix.md
│   │   │   ├── review-pr.md
│   │   │   ├── spike.md
│   │   │   └── verify.md
│   │   ├── agents/            # Global agents (4 files)
│   │   │   ├── build-error-resolver.md
│   │   │   ├── code-reviewer.md
│   │   │   ├── e2e-runner.md
│   │   │   └── security-reviewer.md
│   │   ├── rules/             # Global rules (4 files)
│   │   │   ├── agents.md
│   │   │   ├── git-workflow.md
│   │   │   ├── hooks.md
│   │   │   └── performance.md
│   │   ├── skills/            # Global skills (1 file)
│   │   │   └── codemap-updater.md
│   │   ├── CLAUDE.md          # Global personal instructions
│   │   ├── .mcp.json.example  # Example MCP configuration
│   │   └── settings.json.example # Example settings
│   └── project/               # .claude/ structure
│       ├── commands/          # Project commands (10 files)
│       │   ├── bug-fix.md
│       │   ├── hotfix.md
│       │   ├── plan.md
│       │   ├── raise-pr.md
│       │   ├── refactor-clean.md
│       │   ├── review-pr.md
│       │   ├── spike.md
│       │   ├── start-ticket.md
│       │   ├── tdd.md
│       │   └── verify.md
│       ├── agents/            # Project agents (8 files)
│       │   ├── architect.md
│       │   ├── build-error-resolver.md
│       │   ├── code-reviewer.md
│       │   ├── database-reviewer.md
│       │   ├── planner.md
│       │   ├── refactor-cleaner.md
│       │   ├── security-reviewer.md
│       │   └── tdd-guide.md
│       ├── rules/              # Project rules (20 files)
│       │   ├── workflows.md
│       │   ├── code-style.md
│       │   ├── README.md
│       │   ├── advanced-features.md
│       │   ├── built-in-features.md
│       │   ├── cli-reference.md
│       │   ├── mcp-servers.md
│       │   ├── common/
│       │   │   ├── git-workflow.md
│       │   │   ├── security.md
│       │   │   └── testing.md
│       │   ├── typescript/
│       │   │   ├── coding-style.md
│       │   │   ├── patterns.md
│       │   │   └── testing.md
│       │   └── infrastructure/
│       │       ├── README.md
│       │       ├── secrets-management.md
│       │       ├── style-verification.md
│       │       ├── terraform.md
│       │       ├── terraform-patterns.md
│       │       ├── VERIFICATION_SUMMARY.md
│       │       └── workflow-examples.md
│       ├── skills/            # Project skills (3 files)
│       │   ├── database-migrations.md
│       │   ├── postgres-patterns.md
│       │   └── example-skill/
│       │       └── SKILL.md
│       ├── hooks/             # Project hooks (3 files)
│       │   ├── pre-tool-use/
│       │   │   ├── dev-server-block.md
│       │   │   └── git-push-reminder.md
│       │   ├── post-tool-use/
│       │   │   └── prettier-format.md
│       │   └── stop/
│       │       └── console-log-check.md
│       ├── WORKFLOWS.md       # Main workflow documentation
│       ├── CLAUDE.local.md    # Project personal instructions
│       ├── .mcp.json.example  # Example MCP configuration
│       └── settings.local.json.example # Example project settings
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

## File Count Summary

- **Global files:** 15 files (commands, agents, rules, skills, configs)
- **Project files:** 47 files (commands, agents, rules, skills, hooks, workflows)
- **Total implementation examples:** 62 files
- **Best practices:** 8 documentation files
- **Workflows:** 3 workflow guides
- **Tips:** 3 troubleshooting guides

All files have been sanitized to remove project-specific paths, names, and sensitive information.

## Contributing

This repository documents my personal Claude Code setup. Feel free to:
- Fork and adapt for your own use
- Suggest improvements via issues
- Share your own patterns and workflows

## License

MIT License - See [LICENSE](LICENSE) file
