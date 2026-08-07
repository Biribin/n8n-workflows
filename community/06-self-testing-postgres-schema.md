<!-- Categorie : Tutorials. Image d'en-tete : capture de la sortie PREUVE idempotence, 1920x1080. -->

# Title

A workflow that proves its own upserts are idempotent, by running every insert twice

# Body

"It's idempotent" is the most repeated untested claim in automation. Every retry, every
webhook redelivery, every re-run after a fixed bug depends on it, and almost nobody checks.

This workflow creates a PostgreSQL schema and then proves the claim in the same run.

## The pattern

1. **Create the schema idempotently.** `CREATE SCHEMA IF NOT EXISTS`, tables, then the
   upsert statements written as `INSERT ... ON CONFLICT DO UPDATE`.
2. **Generate fixtures, and feed each one twice.** Three records, six insert attempts. Three
   decision contexts, six attempts. Two send events, four attempts.
3. **Assert the counts.** One query returns, per object, the rows present, the rows expected
   and the insertions attempted:

   ```sql
   SELECT 'offre' AS objet, count(*) AS lignes, 3 AS attendu, 6 AS insertions_tentees
   FROM career_ops.offre WHERE id LIKE 'zz-test-%'
   UNION ALL ...
   ```

   If `lignes` ever differs from `attendu`, the upsert key is wrong and you found out now
   rather than in production.
4. **Exercise the read side too.** It runs the real "follow-ups pending for more than 7 days"
   query against the fixtures, so the derived columns and the date arithmetic are checked,
   not just the writes.
5. **Clean up.** Every fixture id is prefixed `zz-`, and the last step deletes them and
   reports the remaining row count so you can see the database came back to where it started.

## Two details that make it trustworthy

- **Fixtures are prefixed, not truncated.** `DELETE ... WHERE id LIKE 'zz-%'` cannot
  accidentally erase real rows. Never point a test workflow at `TRUNCATE`.
- **Strip `BEGIN`/`COMMIT` from generated SQL.** n8n's Postgres node manages its own
  transaction, so an explicit one inside the query breaks it. That cost me a while.

## Why in n8n rather than a migration tool

For a real product, use a migration tool. For a side schema owned by the automation itself,
this keeps the definition, the fixtures and the proof in one artifact, versioned with the
workflow that depends on it, with no extra tooling to install.

Workflow JSON:
`workflows/motifs-n8n/postgres-schema-et-auto-test.json` in
https://github.com/Biribin/n8n-workflows
