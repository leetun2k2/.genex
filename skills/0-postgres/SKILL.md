---
name: 0-postgres
description: Connect to PostgreSQL with psql and inspect schemas, data, query plans, and locks to test or debug connection and query issues.
---

# PostgreSQL

## Installation

If a required CLI or runtime is missing, read [installation and verification](references/installation.md).


Use the installed `psql`. Choose commands based on the user's question; use `psql --help` or `psql --help=commands` when needed.

## Read and write policy

- Run in-scope read-only operations without user confirmation. Keep filters, limits, and timeouts as operational safeguards.
- Before any write, mutation, or configuration change, including local edits, prepare a concrete change set. Immediately before execution, show the target, exact commands with secrets redacted, and a diff or effects summary when no text diff is available. Ask for explicit user confirmation and wait for it. The initial task request alone does not satisfy this checkpoint.
- Confirmation covers the displayed change set, including repetitive sub-steps within it. Ask again only if the target, proposed changes, or side effects change.

## Inputs

Resolve these from the request, environment, or project config. Ask only when a missing fact prevents useful work.

| Input | Details |
| --- | --- |
| Target | Environment, host/port or socket, database, user, credentials, and TLS if needed. Reuse `PGHOST`, `PGPORT`, `PGDATABASE`, `PGUSER`, or a configured service. |
| Goal | What to test/debug, with schema/table, row filters, SQL, or expected behavior when relevant. |
| Write scope | For writes: allowed target, operation, and rows/schema. Inspection is read-only by default. |

## Usage

- Reuse existing credentials. Prefer `.pgpass` or `PGPASSFILE` with mode `0600`; reuse an existing `PGPASSWORD` if supplied. Keep secrets out of command arguments, logs, and committed files. Preserve required TLS verification.
- Use `-X -w -v ON_ERROR_STOP=1 -P pager=off` for agent runs: skip startup files, avoid password prompts, stop on errors, and disable the pager.
- Set connection, statement, and lock timeouts suited to the task. Use the execution runner's deadline for total runtime; `PGCONNECT_TIMEOUT` only bounds connection attempts.
- Inspect the schema before writing queries. Use schema-qualified names, selected columns, filters, and small limits. `LIMIT` bounds rows returned, not query cost or payload size. Use read-only transactions or session defaults for inspection.
- Start with plain `EXPLAIN`. `EXPLAIN ANALYZE` executes the query and needs the same authorization as that query. Check functions and CTEs for side effects; wrapping a write in `ROLLBACK` does not make it read-only or undo every effect.
- Include all mutating SQL, maintenance, configuration changes, and session termination in the write confirmation checkpoint. Show the exact SQL with secrets redacted, affected tables/row filters, and expected row/schema effects before execution; a direct request or planned rollback does not replace confirmation.
- Use psql variables with `:'value'` for SQL literals and `:"identifier"` for identifiers. Variable interpolation works in stdin or `-f` scripts, not SQL passed through `-c`; avoid shell-built SQL.
- Report the target without secrets, commands/SQL used, key results, and any limits. Separate findings from possible causes; stop when the question is answered.

## Examples

Adapt queries to the inspected schema. These examples use macOS zsh/bash and assume connection variables and credentials are already resolved.

```sh
# Inspection defaults; preserve any existing connection options.
export PGCONNECT_TIMEOUT=5
export PGOPTIONS="${PGOPTIONS:+$PGOPTIONS }-c default_transaction_read_only=on -c statement_timeout=5000 -c lock_timeout=1000"
pg_cmd=(psql -X -w -v ON_ERROR_STOP=1 -P pager=off)

# Verify the connection and role.
"${pg_cmd[@]}" -c 'SELECT current_database(), current_user, pg_is_in_recovery();'

# Inspect schemas, tables, columns, and indexes.
"${pg_cmd[@]}" -c '\dn'
"${pg_cmd[@]}" -c '\dt public.*'
"${pg_cmd[@]}" -c '\d+ public.orders'

# Read a row and inspect its query plan without executing EXPLAIN ANALYZE.
"${pg_cmd[@]}" -v order_id=123 <<'SQL'
SELECT id, status FROM public.orders WHERE id = :'order_id' LIMIT 20;
EXPLAIN SELECT id, status FROM public.orders WHERE id = :'order_id';
SQL

# Find sessions waiting on a lock; omit potentially sensitive query text.
"${pg_cmd[@]}" -c "SELECT pid, state, wait_event, pg_blocking_pids(pid) AS blockers
FROM pg_stat_activity WHERE wait_event_type = 'Lock' LIMIT 20;"
```

For a write test, first show the target, SQL, expected effects, and cleanup and receive explicit user confirmation. Use a separate session with the required write settings. Rollback cannot restore sequence values or reverse external effects.

Example request: “Use staging config to inspect `public.orders` and explain why the query for order 123 is slow. Start with its plan and relevant indexes.”

References: [psql](https://www.postgresql.org/docs/current/app-psql.html), [password files](https://www.postgresql.org/docs/current/libpq-pgpass.html), [EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html).
