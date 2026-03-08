---
paths:
  - "**/*.ts"
  - "**/*.tsx"
---
# TypeScript Coding Style

## Formatting
- `semi: false` — no semicolons
- `singleQuote: true`
- `tabWidth: 2`
- `printWidth: 100`
- Run `npm run lint:fix` and `npm run format` before committing

## ESM Only
All Node packages use `"type": "module"`. Never use `require()`:

```typescript
// WRONG
const fs = require('fs')

// CORRECT
import fs from 'fs'
```

## Immutability
Prefer spread over mutation:

```typescript
// WRONG
user.name = name

// CORRECT
return { ...user, name }
```

## Error Handling
Use async/await with try-catch:

```typescript
try {
  const result = await riskyOperation()
  return result
} catch (error) {
  console.error('Operation failed:', error)
  throw new Error('User-friendly message')
}
```

## No console.log in Production
Use a proper logger. Remove all `console.log` before committing.

## Path Aliases (platform-app)
- `@/*` → `src/*`
- `@assets/*` → `assets/*`

## Type Safety
- No `any` unless absolutely necessary — document why if used
- No `as unknown as X` double-cast to bypass type safety
- Use `satisfies` for type narrowing instead of casting

## Strict Mode (platform-app)
The mobile app uses strict TypeScript.
