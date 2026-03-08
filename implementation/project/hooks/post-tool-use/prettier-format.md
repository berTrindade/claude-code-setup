# Prettier Format Hook

Automatically formats code files after Edit or Write operations.

**Matcher:** `Edit`, `Write`
**Hook Type:** PostToolUse
**Command:**
```javascript
node -e "let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{try{const i=JSON.parse(d);const f=i.tool_input?.file_path||'';if(f.match(/\.(ts|tsx|js|jsx|json|css)$/)){const{execSync}=require('child_process');const path=require('path');try{execSync('npx prettier --write '+JSON.stringify(path.resolve(f)),{stdio:'pipe',timeout:10000});console.error('[Hook] Formatted: '+f)}catch{console.error('[Hook] Prettier skipped: '+f)}}}catch{}console.log(d)})"
```
**Purpose:** Automatically formats TypeScript, JavaScript, JSON, and CSS files after edits.
