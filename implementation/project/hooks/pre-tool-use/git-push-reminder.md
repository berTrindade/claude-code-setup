# Git Push Reminder Hook

Reminds to review changes before pushing to remote.

**Matcher:** `Bash`

**Hook Type:** PreToolUse

**Command:**
```javascript
node -e "let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{try{const i=JSON.parse(d);const cmd=i.tool_input?.command||'';if(/git push/.test(cmd)){console.error('[Hook] Reminder: review your changes before pushing')}}catch{}console.log(d)})"
```

**Purpose:** Provides a reminder to review changes before pushing to prevent accidental pushes.
