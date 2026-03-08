# Hooks Reference

Documents all active hooks so Claude understands what fires automatically and avoids duplicating behaviour.

## Hook Structure

Hooks are now organized in the `.claude/hooks/` directory structure:

- **PreToolUse hooks:** `.claude/hooks/pre-tool-use/*.md`
- **PostToolUse hooks:** `.claude/hooks/post-tool-use/*.md`
- **Stop hooks:** `.claude/hooks/stop/*.md`

## Project Hooks (`.claude/hooks/`)

### PreToolUse

| Hook File | Trigger | What It Does |
|-----------|---------|-------------|
| `pre-tool-use/dev-server-block.md` | Any `Bash` command matching `npm run dev`, `pnpm dev`, `yarn dev`, `bun run dev` | **Blocks** the command and prints instructions to use tmux instead |
| `pre-tool-use/git-push-reminder.md` | Any `Bash` command matching `git push` | Prints a reminder to review changes before pushing (non-blocking) |

### PostToolUse

| Hook File | Trigger | What It Does |
|-----------|---------|-------------|
| `post-tool-use/prettier-format.md` | `Edit` or `Write` on `.ts`, `.tsx`, `.js`, `.jsx`, `.json`, `.css` files | Runs Prettier on the edited/written file |

### Stop (end of session)

| Hook File | Trigger | What It Does |
|-----------|---------|-------------|
| `stop/console-log-check.md` | Always | Scans all git-modified `.ts/.tsx/.js/.jsx` files for `console.log` and reports locations |

## Global Hooks (`~/.claude/settings.json`)

Global hooks are configured in `~/.claude/settings.json` if needed. See that file for global hook definitions.

## What Hooks Do NOT Cover (handle manually)

- Running tests before commit — do this explicitly with `npm test`
- Formatting non-TS files (CSS, JSON, YAML) — run Prettier manually if needed
- Linting — run `npm run lint:fix` before committing
