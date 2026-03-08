# Security Guidelines

## Mandatory Checks Before Any Commit

- [ ] No hardcoded secrets (API keys, passwords, tokens, connection strings)
- [ ] All user inputs validated with Zod schemas
- [ ] SQL injection prevention — parameterized queries only
- [ ] XSS prevention — user content sanitized
- [ ] Authentication verified on all protected routes (Cognito JWT)
- [ ] Rate limiting on all API endpoints
- [ ] Error messages don't leak stack traces or internal details
- [ ] No sensitive data (PII, tokens) in `console.log`

## Secret Management

```typescript
// NEVER
const apiKey = 'sk-proj-xxxxx'

// ALWAYS
const apiKey = process.env.LAB_API_KEY
if (!apiKey) throw new Error('LAB_API_KEY not configured')
```

If a security issue is found: stop, use the `security-reviewer` agent, fix CRITICAL issues first, rotate any exposed secrets.
