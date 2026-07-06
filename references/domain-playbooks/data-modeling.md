# Playbook — Data modeling and databases

The schema outlives every app rewrite. Errors here cost 100x downstream —
which is why schema changes are `best`-model work while queries are not.

## Pipeline

1. **Model the domain, not the UI (best):** entities, relationships,
   cardinality (1:1 / 1:N / M:N), and lifecycle states — on paper before DDL.
   For every entity: what uniquely identifies it, what can change, what
   must never change after creation.
2. **Schema rules (best writes, Sonnet implements):** see rules below.
3. **Migrations (Sonnet):** every schema change is a migration file, both
   directions tested (see distributed-systems.md). Never edit a shipped
   migration; add a new one.
4. **Query layer (Sonnet/Haiku):** parameterized always; each new query
   EXPLAIN'd against realistic row counts before merge.
5. **Verify (Haiku, rubric below).**

## Schema rules (defaults; deviations need a decisions/ entry)

- Normalize until it hurts (3NF), denormalize only with a measured reason
  written down. Duplicate data = future inconsistency with a timer on it.
- Every table: opaque PK (UUIDv7 > random UUIDv4 for index locality),
  `created_at`/`updated_at` (UTC, timezone-aware type), and `user_id`/tenant
  key on every user-owned table — retrofitting tenancy is a rewrite.
- Soft delete (`deleted_at`) for user-visible business entities; hard delete
  for logs/derived data. Pick per table, write it down.
- Constraints in the DATABASE, not just the app: FKs, NOT NULL, UNIQUE,
  CHECK. The DB is the last line of defense and the only one that holds
  when a second writer appears.
- Enums: DB-native enums or lookup tables with FK — never bare strings
  checked only in code.
- Money: integer minor units + currency column. Time: always UTC,
  timezone-aware column types. Floats never hold money or IDs.
- Names: `snake_case`, singular or plural but ONE convention, no
  abbreviations (`quantity` not `qty`).

## Indexing (procedure, not vibes)

- Index: every FK, every column in frequent WHERE/ORDER BY, every UNIQUE
  business key. Composite indexes ordered by (equality columns first, then
  range column); a composite serves its prefix, so (a,b) makes (a) redundant.
- Don't index: tiny tables, low-cardinality flags alone, columns only in
  SELECT. Every index taxes every write — additions beyond the defaults
  cite an EXPLAIN.
- The N+1 check is mandatory: any code that queries inside a loop over
  query results is a defect — batch it (JOIN, IN-list, or dataloader).

## Verification rubric (binary)

- [ ] ER sketch (entities/cardinalities/states) exists in docs/ before DDL
      (check dates)
- [ ] Every table has PK, timestamps, tenancy key (or written N/A why)
- [ ] All FKs declared with explicit ON DELETE behavior (no accidental
      CASCADE — each one is a decision)
- [ ] NOT NULL/UNIQUE/CHECK constraints in DDL for every rule the app
      enforces (grep app validations, cross-check)
- [ ] Migration up + down both run clean on a fresh DB
- [ ] Every FK has an index; no query in a result-loop (grep for the
      pattern)
- [ ] New queries EXPLAIN'd at realistic scale; no seq-scan on large-table
      hot paths; evidence in ledger
- [ ] No raw string interpolation into SQL anywhere (grep) — parameterized
      only

## Known failure modes

1. **UI-shaped schema.** Tables mirroring today's screens; second feature
   requires migration hell. Model the domain (step 1).
2. **App-only constraints.** "The code checks it" — until the second writer
   (script, admin panel, migration) doesn't. DB constraints rule.
3. **Missing tenancy key.** Single-user assumption fossilized into every
   table. One column now vs a rewrite later.
4. **Index cargo-culting.** Indexing everything (write amplification) or
   nothing (seq scans). The procedure above, not instinct.
5. **Editing shipped migrations.** Breaks every environment that already
   ran them. Append-only, forever.
6. **Timezone-naive timestamps.** Works until the first non-UTC server or
   DST boundary. Timezone-aware types, UTC, always.
