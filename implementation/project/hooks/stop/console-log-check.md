# Console.log Check Hook

Checks for console.log statements in modified files when session stops.

**Matcher:** `*`

**Hook Type:** Stop

**Command:**
```javascript
node -e "const{execSync}=require('child_process');try{const files=execSync('git diff --name-only HEAD 2>/dev/null',{encoding:'utf8'}).trim().split('\n').filter(f=>f.match(/\.(ts|tsx|js|jsx)$/));const found=[];files.forEach(f=>{try{const c=require('fs').readFileSync(f,'utf8');const lines=c.split('\n');lines.forEach((l,i)=>{if(/console\.log/.test(l))found.push(f+':'+(i+1))});}}catch{});if(found.length>0){console.error('[Hook] console.log found in modified files:');found.forEach(l=>console.error('  '+l));}}catch{}"
```

**Purpose:** Warns about console.log statements in modified files to encourage proper logging practices.
