---
paths:
  - "**/*.ts"
  - "**/*.tsx"
---
# TypeScript Testing

Extends `rules/common/testing.md` with TypeScript-specific patterns.

## Test File Naming

```
src/
  services/
    care-plan.service.ts
    care-plan.service.test.ts     # unit tests alongside source
  routes/
    pets.route.ts
    pets.route.test.ts            # integration tests
e2e/
  lab-results.spec.ts             # E2E critical flows
```

## Mocking in ESM

```typescript
// For ESM modules, use jest.unstable_mockModule or vitest's vi.mock
import { vi } from 'vitest'

vi.mock('../db/client', () => ({
  db: {
    query: vi.fn().mockResolvedValue({ rows: [] }),
  },
}))
```

## Type-Safe Test Helpers

```typescript
// Use typed factories instead of casting
function createPet(overrides: Partial<Pet> = {}): Pet {
  return {
    id: 'pet-123',
    name: 'Buddy',
    species: 'dog',
    ownerId: 'user-456',
    ...overrides,
  }
}
```

## Agent Support

- **tdd-guide** — use proactively for new features
- **build-error-resolver** — when TypeScript errors prevent tests from running
