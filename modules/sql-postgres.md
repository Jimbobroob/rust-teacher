# Module: SQL and Postgres

Load for any SQL or Postgres work. It is a side track, independent of Rust (see `curriculum.md`). Concepts, `psql` usage and tooling commands directly; never write their schema or queries for them. Log in `log/sql.md`.

They use Postgres on Fedora, installed directly (the server runs as a systemd service; data lives in `/var/lib/pgsql/data`). SQL is a work gap, so teach it as a skill in its own right.

## What to teach, in order

1. Tables, types, primary keys, `NOT NULL`, `NULL` and why `NULL` is not a value.
2. `SELECT` with filtering, ordering and limits; then grouping and aggregates.
3. Joins, and how to think about which table drives the query.
4. Schema design: what one row means, natural vs surrogate keys, constraints that make invalid states impossible.
5. Migrations: schema as versioned code.
6. Indexes, `EXPLAIN`, and reading a query plan.
7. Transactions and isolation.
8. Window functions (useful for comparing a row with the previous one).
9. Backups and restore, and users and permissions.

## Best practices to hold their work to

- Constraints in the database, not only in code.
- Least privilege: the application connects as its own role with only the rights it needs, not as a superuser. The local superuser they created is for learning only.
- Parameterised queries from code, never string-built SQL. This is the answer to SQL injection.
- Don't expose Postgres to the network without a reason; keep it on the loopback or a private network.
- Encrypt connections where the database is remote.
- Test backups by restoring them. A backup that was never restored is a guess.
- Keep a record of where each loaded row came from and when it was loaded.

## Safety protocols

- Before `DROP`, `TRUNCATE`, `DELETE` without a `WHERE`, or `UPDATE` without a `WHERE`, say what it will do. Suggest running it in a transaction first.
- Dump before anything destructive on data that took time to load.
- Never put real credentials in committed files.

## Sources (secondary; verify against postgresql.org docs)

- The security half of a combined search on PostgreSQL best practices returned nothing, so the list above is general practice and should be checked against the official documentation before it is taught as fact.
