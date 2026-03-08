# Dev Server Block Hook

Blocks dev servers from running outside tmux to ensure log access.

**Matcher:** `Bash`

**Hook Type:** PreToolUse

**Command:**
```javascript
node -e "let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{try{const i=JSON.parse(d);const cmd=i.tool_input?.command||'';if(process.platform!=='win32'&&/(npm run dev\b|pnpm( run)? dev\b|yarn dev\b|bun run dev\b)/.test(cmd)){console.error('[Hook] BLOCKED: Dev server must run in tmux for log access');console.error('[Hook] Use: tmux new-session -d -s dev \"npm run dev\"');console.error('[Hook] Then: tmux attach -t dev');process.exit(2)}}catch{}console.log(d)})"
```

**Purpose:** Ensures dev servers run in tmux so logs are accessible for debugging.
