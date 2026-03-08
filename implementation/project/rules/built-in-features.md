# Built-in Claude Code Features

Documentation of built-in Claude Code features and how to use them.

## Checkpointing

Automatic tracking of file edits with rewind capability.

### Usage

- **Rewind:** Press `Esc Esc` or use `/rewind` command
- **When to use:** When Claude goes off-track or makes unwanted changes
- **Alternative:** Use `/clear` to reset context mid-session if switching to a new task

### How It Works

- Automatically tracks file edits via git
- Creates checkpoints at significant changes
- Allows targeted summarization of changes

**Best Practice:** Use checkpointing instead of trying to fix issues in the same context.

## Voice Mode

Speak-to-prompt functionality for hands-free interaction.

### Usage

- **Activate:** Use `/voice` command
- **When useful:** When you want to describe issues verbally or work hands-free
- **Deactivate:** Use `/voice` again or exit voice mode

### Benefits

- Faster input for complex descriptions
- Natural language interaction
- Hands-free operation

## Remote Control

Continue local sessions from any device — phone, tablet, or browser.

### Usage

- **Activate:** Use `/remote-control` command
- **Headless mode:** Run Claude Code in headless mode for remote access
- **Continue sessions:** Access your session from any device with network access

### Use Cases

- Continue work from mobile device
- Share session with team member
- Monitor long-running tasks remotely

## Git Worktrees

Isolated git branches for parallel development — each agent gets its own working copy.

### Usage

- **Create worktree:** `git worktree add <path> <branch>`
- **List worktrees:** `git worktree list`
- **Remove worktree:** `git worktree remove <path>`

### Benefits

- Parallel development on multiple branches
- Isolation between features
- No branch switching needed
- Each agent can work in its own worktree

### Best Practices

- Use with agent teams for parallel development
- Clean up worktrees when done: `git worktree remove <path>`
- Use tmux for managing multiple worktree sessions

## Ralph Wiggum Loop

Autonomous development loop for long-running tasks — iterates until completion.

### Installation

Install the Ralph Wiggum plugin:
```bash
claude plugin install ralph-wiggum
```

### Usage

- **When to use:** Long-running autonomous tasks that need iteration
- **How it works:** Agent iterates on a task until completion criteria are met
- **Configuration:** Set completion criteria and iteration limits

### Use Cases

- Refactoring large codebases
- Migrating between frameworks
- Automated test writing
- Documentation generation

### Best Practices

- Set clear completion criteria
- Monitor iteration progress
- Use for well-defined, repetitive tasks
- Not suitable for tasks requiring human judgment
