---
paths:
  - "**/*.ts"
  - "**/*.tsx"
---
# TypeScript Patterns

## API Response Format

```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: {
    total: number
    cursor: string | null
  }
}

// Usage
res.json({ success: true, data: carePlan } satisfies ApiResponse<CarePlan>)
```

## Repository Pattern

```typescript
interface Repository<T, CreateDto, UpdateDto> {
  findAll(filters?: Record<string, unknown>): Promise<T[]>
  findById(id: string): Promise<T | null>
  create(data: CreateDto): Promise<T>
  update(id: string, data: UpdateDto): Promise<T>
  delete(id: string): Promise<void>
}
```

## Custom Hook Pattern (platform-app / platform-website)

```typescript
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)
  useEffect(() => {
    const handler = setTimeout(() => setDebouncedValue(value), delay)
    return () => clearTimeout(handler)
  }, [value, delay])
  return debouncedValue
}
```

## ESM Import Rules

```typescript
// WRONG: CommonJS
const { z } = require('zod')

// CORRECT: ESM named import
import { z } from 'zod'

// For local files — include extension when required by Node ESM resolution
import { buildCarePlan } from './care-plan-builder.js'
```

## Immutability

```typescript
// WRONG: mutation
user.name = newName

// CORRECT: spread
return { ...user, name: newName }

// WRONG: push
results.push(newItem)

// CORRECT: map/filter/concat
return [...results, newItem]
```

## Error Types

```typescript
export class ApiError extends Error {
  constructor(
    public readonly statusCode: number,
    message: string
  ) {
    super(message)
    this.name = 'ApiError'
  }
}

export class NotFoundError extends ApiError {
  constructor(resource: string) {
    super(404, `${resource} not found`)
  }
}
```
