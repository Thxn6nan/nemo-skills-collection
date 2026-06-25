---
name: database-maintenance
description: >
  Plan and execute database operations safely — schema changes, migrations,
  query optimization, index management, backup verification, and capacity
  planning. Use this skill when designing a schema, planning a migration,
  diagnosing slow queries, managing database health, or preparing for data
  growth. Trigger phrases: "design this schema", "plan this migration",
  "slow query", "optimize this query", "add an index", "database is getting
  slow", "how do we migrate this safely?", "database capacity planning",
  "backup strategy", "database maintenance plan".
---

# Database Maintenance Skill

Covers the full lifecycle of database work: design, migration, optimization,
and ongoing health. Databases are the hardest things to fix when broken —
invest in doing them right the first time.

## Core Principle

> A database schema is a commitment. Changing it in production is expensive.
> A migration that fails at 3 AM on live data is expensive.
> A slow query that appears only at scale is expensive.
> All three are cheaper to prevent than to fix.

---

## Domain 1: Schema Design

### Design Principles
```
NORMALIZATION (prevent data anomalies):
  □ Each table represents one entity or relationship
  □ No repeating groups within a row
  □ Non-key fields depend on the whole key (not just part of it)
  □ No transitive dependencies (A → B → C belongs in a separate table)
  
  When to denormalize: read performance is critical AND writes are infrequent
  Always document why you denormalized — it's a deliberate trade-off

NAMING CONVENTIONS (pick one, document it, never mix):
  □ Table names: singular or plural (but consistent)
  □ Column names: snake_case or camelCase (but consistent)
  □ Primary keys: id or table_name_id (but consistent)
  □ Foreign keys: referenced_table_id (self-documenting)
  □ Timestamps: created_at, updated_at (standard names everyone knows)

DATA TYPES:
  □ Use the most restrictive type that fits the data
  □ NOT NULL on every column unless null has explicit meaning
  □ VARCHAR: set realistic max length (not VARCHAR(255) for everything)
  □ DECIMAL for money (never FLOAT — floating point loses cents)
  □ TIMESTAMP WITH TIMEZONE for any datetime (avoid timezone bugs)
  □ UUID vs auto-increment: UUID for distributed systems, auto-increment for simplicity

CONSTRAINTS (enforce at the database level, not just application):
  □ PRIMARY KEY on every table
  □ FOREIGN KEY constraints (prevents orphaned records)
  □ UNIQUE constraints where values must be unique
  □ CHECK constraints for value ranges or enums
  □ DEFAULT values where sensible
```

---

## Domain 2: Migration Planning

**Migrations are the highest-risk database operation. Plan everything.**

### Migration Safety Checklist

```
BEFORE WRITING THE MIGRATION:
  □ Can this be done without downtime? (see patterns below)
  □ How long will it run? (test on production-sized data copy)
  □ Does it lock tables? (ALTER TABLE locks in some databases)
  □ Is there a rollback migration? (written and tested)
  □ Is there a backup taken immediately before running?

MIGRATION PATTERNS (safest to most dangerous):
  ✅ Add nullable column → safe, no locks, instant
  ✅ Add new table → safe
  ✅ Add index CONCURRENTLY (Postgres) → safe, no locks
  ⚠️ Add NOT NULL column → requires default value or two-step migration
  ⚠️ Rename column → requires two deployments (add new, migrate data, drop old)
  ⚠️ Change column type → may require rewrite of entire table
  ❌ DROP column/table → irreversible; use soft delete first (rename with _deprecated)
  ❌ ALTER TABLE (MySQL without ALGORITHM=INPLACE) → locks entire table

ZERO-DOWNTIME MIGRATION PATTERN (for breaking changes):
  Step 1: Add new column/table (backward compatible)
  Step 2: Deploy code that writes to both old and new
  Step 3: Backfill old data into new structure
  Step 4: Verify backfill complete and correct
  Step 5: Deploy code that reads from new only
  Step 6: Drop old column/table (after confirming nothing reads it)
```

### Migration Execution

```
BEFORE RUNNING IN PRODUCTION:
  □ Run migration on a copy of production data (same size)
  □ Measure execution time on that copy
  □ Test rollback migration
  □ Schedule during lowest-traffic window
  □ Confirm backup exists and is recent
  □ Have rollback ready to execute immediately

DURING MIGRATION:
  □ Monitor: query execution time, lock waits, replication lag
  □ Have a "stop" criteria: if X happens, abort and rollback
  □ Don't run anything else on the database during a heavy migration

AFTER MIGRATION:
  □ Verify row counts match expectations
  □ Run application smoke tests
  □ Monitor error rates for 30 minutes post-migration
  □ Verify indexes were created correctly
```

---

## Domain 3: Query Optimization

### Diagnosis First

```
FIND SLOW QUERIES:
  PostgreSQL: pg_stat_statements, EXPLAIN ANALYZE
  MySQL:      slow query log, EXPLAIN
  MongoDB:    db.setProfilingLevel(2), explain()

READ THE QUERY PLAN:
  Look for:
    □ Sequential scan (Seq Scan) on a large table → missing index
    □ Nested loop with large outer set → may need different join strategy
    □ Hash join spilling to disk → work_mem too low or query too broad
    □ Row count estimates wildly off → statistics need updating (ANALYZE)

BEFORE ADDING AN INDEX:
  □ Does the query actually need to be faster? (is this a real problem?)
  □ Does the column appear in WHERE, JOIN ON, or ORDER BY?
  □ What is the cardinality? (high cardinality = better index selectivity)
  □ How often is this table written to? (indexes slow writes)
  □ Is this a composite index candidate? (WHERE a = X AND b = Y)
```

### Common Query Optimization Patterns

```
N+1 QUERY PROBLEM:
  Symptom: 1 query to get N records, then N queries to get related data
  Fix:     JOIN or batch fetch in one query; use eager loading in ORM

MISSING INDEX:
  Symptom: Seq Scan on large table in query plan
  Fix:     Add index on the column(s) in WHERE/JOIN/ORDER BY

OVER-FETCHING:
  Symptom: SELECT * when only 2 columns are needed
  Fix:     SELECT only the columns needed; reduces I/O and network

PAGINATION WITHOUT INDEX:
  Symptom: OFFSET 10000 LIMIT 20 gets slower as offset grows
  Fix:     Keyset pagination: WHERE id > last_seen_id LIMIT 20

UNINDEXED SORT:
  Symptom: ORDER BY on column with no index → full sort in memory
  Fix:     Add index on the sort column (or composite with filter columns)

FUNCTION ON INDEXED COLUMN:
  Symptom: WHERE LOWER(email) = 'x' doesn't use email index
  Fix:     Use a functional index: CREATE INDEX ON users (LOWER(email))
```

---

## Domain 4: Ongoing Health

### Index Maintenance
```
□ Remove unused indexes (they slow writes with no read benefit)
  PostgreSQL: pg_stat_user_indexes WHERE idx_scan = 0
□ Rebuild fragmented indexes periodically
□ Review index usage monthly as query patterns change
```

### Capacity Planning
```
□ Track database size growth rate (GB/month)
□ Project when storage will be exhausted (current rate × headroom)
□ Monitor connection pool usage (alert at 80% utilization)
□ Track query performance trend (are queries getting slower over time?)
□ Partition large tables before they become unmanageable
   (typically: >100M rows or >100GB in a single table)
```

### Backup Verification
```
□ Backups run on schedule (verify, don't assume)
□ Backup restoration tested monthly (verify it actually works)
□ Point-in-time recovery tested (can you restore to 2 hours ago?)
□ Backup stored separately from primary (not on same server)
□ Retention policy: how many days/weeks of backups are kept?
□ Recovery time objective (RTO): how long does restore take? Is that acceptable?
```

---

## Reference Files

- `references/migration-patterns.md` — Safe migration patterns with examples
