# Code Style

- TypeScript across all packages; strict mode in the mobile app
- ESLint + Prettier: `semi: false`, `singleQuote: true`, `tabWidth: 2`, `printWidth: 100`
- ESM only — all Node packages use `"type": "module"`
- No default exports in backend services; named exports preferred
- Environment variables follow `.env.example` templates in each package
- Branch naming: `feature/`, `fix/`, `refactor/` prefixes
- Architectural decisions documented in `docs/adr/`

## Writing Style Guidelines

- **Text style**: Use casual tone, avoid em dashes, avoid colons in descriptive text
- **Comments**: Keep comments concise, usually 1 line when needed
  - Use casual tone
  - Avoid em dashes
  - Avoid colons where possible
  - Only add comments when they add value or clarify non-obvious behavior
