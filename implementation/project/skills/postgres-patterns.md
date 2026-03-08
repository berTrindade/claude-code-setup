# PostgreSQL Patterns

Quick reference for PostgreSQL best practices. Use with the `database-reviewer` agent.

## Index Cheat Sheet

| Query Pattern | Index Type | Example |
|---|---|---|
| `WHERE col = value` | B-tree (default) | `CREATE INDEX idx ON t (col)` |
| `WHERE a = x AND b > y` | Composite | `CREATE INDEX idx ON t (a, b)` |
| `WHERE col IS NULL` | Partial | `CREATE INDEX idx ON t (col) WHERE col IS NULL` |
| `WHERE deleted_at IS NULL` | Partial | `CREATE INDEX idx ON t (id) WHERE deleted_at IS NULL` |
| Full-text search | GIN | `CREATE INDEX idx ON t USING gin (col)` |

## Data Type Quick Reference

| Use Case | Correct Type | Avoid |
|---|---|---|
| IDs | `bigint` or `uuid` | `int` |
| Strings | `text` | `varchar(255)` |
| Timestamps | `timestamptz` | `timestamp` |
| Money | `numeric(10,2)` | `float` |
| Flags | `boolean` | `int`, `varchar` |

## Common Patterns

**Composite Index — equality first, then range:**
```sql
CREATE INDEX idx ON lab_results (pet_id, collected_at);
-- Works for: WHERE pet_id = $1 AND collected_at > $2
```

**Covering Index — avoids table lookup:**
```sql
CREATE INDEX idx ON pets (owner_id) INCLUDE (name, species);
```

**Partial Index — for soft deletes:**
```sql
CREATE INDEX idx ON pets (owner_id) WHERE deleted_at IS NULL;
```

**UPSERT:**
```sql
INSERT INTO care_actions (pet_id, action_type, recommended_at)
VALUES ($1, $2, NOW())
ON CONFLICT (pet_id, action_type)
DO UPDATE SET recommended_at = EXCLUDED.recommended_at;
```

**Cursor Pagination (never use OFFSET on large tables):**
```sql
SELECT * FROM lab_results WHERE pet_id = $1 AND id > $last_id ORDER BY id LIMIT 20;
```

**Queue Processing with SKIP LOCKED:**
```sql
UPDATE lab_jobs SET status = 'processing'
WHERE id = (
  SELECT id FROM lab_jobs WHERE status = 'pending'
  ORDER BY created_at LIMIT 1
  FOR UPDATE SKIP LOCKED
) RETURNING *;
```

## Anti-Pattern Detection

```sql
-- Find unindexed foreign keys (run these diagnostics)
SELECT conrelid::regclass, a.attname
FROM pg_constraint c
JOIN pg_attribute a ON a.attrelid = c.conrelid AND a.attnum = ANY(c.conkey)
WHERE c.contype = 'f'
  AND NOT EXISTS (
    SELECT 1 FROM pg_index i
    WHERE i.indrelid = c.conrelid AND a.attnum = ANY(i.indkey)
  );

-- Find slow queries
SELECT query, mean_exec_time, calls
FROM pg_stat_statements
WHERE mean_exec_time > 100
ORDER BY mean_exec_time DESC;
```

## Configuration

```sql
ALTER SYSTEM SET idle_in_transaction_session_timeout = '30s';
ALTER SYSTEM SET statement_timeout = '30s';
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
SELECT pg_reload_conf();
```

## Related

- Agent: `database-reviewer` — full database review
- Skill: `/database-migrations` — safe schema changes
