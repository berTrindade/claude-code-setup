# Database Migrations

Safe, reversible database schema changes for production systems.

## Core Principles

1. Every change is a migration — never alter production databases manually
2. Migrations are forward-only in production — rollbacks use new forward migrations
3. Schema and data migrations are separate — never mix DDL and DML in one migration
4. Test migrations against production-sized data
5. Migrations are immutable once deployed — never edit a migration that has run

## Migration Safety Checklist

Before applying any migration:

- [ ] New columns are nullable or have a default
- [ ] Indexes created with `CONCURRENTLY`
- [ ] No full table locks on large tables
- [ ] Data backfill is a separate migration from schema change
- [ ] Tested against a copy of production data
- [ ] Rollback plan documented

## PostgreSQL Patterns

**Adding a column safely:**
```sql
-- GOOD: nullable column, instant
ALTER TABLE pets ADD COLUMN avatar_url TEXT;

-- GOOD: with default (Postgres 11+ is instant)
ALTER TABLE lab_results ADD COLUMN is_flagged BOOLEAN NOT NULL DEFAULT false;

-- BAD: NOT NULL without default — locks table and rewrites all rows
ALTER TABLE pets ADD COLUMN status TEXT NOT NULL;
```

**Adding an index without downtime:**
```sql
-- BAD: blocks writes on large tables
CREATE INDEX idx_lab_results_pet_id ON lab_results (pet_id);

-- GOOD: non-blocking
CREATE INDEX CONCURRENTLY idx_lab_results_pet_id ON lab_results (pet_id);
-- Note: cannot run inside a transaction block
```

**Renaming a column (expand-contract pattern):**
```sql
-- Migration 001: add new column
ALTER TABLE pets ADD COLUMN display_name TEXT;

-- Migration 002: backfill (separate data migration)
UPDATE pets SET display_name = name WHERE display_name IS NULL;

-- Deploy: app reads/writes both columns

-- Migration 003: drop old column (after app updated)
ALTER TABLE pets DROP COLUMN name;
```

**Large data migration (batch update):**
```sql
-- BAD: locks entire table
UPDATE lab_results SET normalized = true;

-- GOOD: batch with SKIP LOCKED
DO $$
DECLARE rows_updated INT;
BEGIN
  LOOP
    UPDATE lab_results SET normalized = true
    WHERE id IN (
      SELECT id FROM lab_results WHERE normalized IS NULL LIMIT 10000 FOR UPDATE SKIP LOCKED
    );
    GET DIAGNOSTICS rows_updated = ROW_COUNT;
    EXIT WHEN rows_updated = 0;
    COMMIT;
  END LOOP;
END $$;
```

## Zero-Downtime Strategy (Expand-Contract)

```
Phase 1: EXPAND
  - Add new column/table (nullable or with default)
  - Deploy: app writes to BOTH old and new
  - Backfill existing data

Phase 2: MIGRATE
  - Deploy: app reads from NEW, writes to BOTH
  - Verify data consistency

Phase 3: CONTRACT
  - Deploy: app only uses NEW
  - Drop old column/table in separate migration
```

## Anti-Patterns

| Anti-Pattern | Why It Fails | Better Approach |
|---|---|---|
| Manual SQL in production | No audit trail | Always use migration files |
| Editing deployed migrations | Drift between environments | Create new migration |
| NOT NULL without default | Locks table, rewrites all rows | Add nullable, backfill, add constraint |
| Inline index on large table | Blocks writes | `CREATE INDEX CONCURRENTLY` |
| Schema + data in one migration | Hard to rollback | Separate migrations |
| Dropping column before removing code | App errors | Remove code first, drop column next deploy |
