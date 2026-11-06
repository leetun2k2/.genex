---
name: 1-database-review
description: Review database changes and affected data access for consequential correctness, integrity, concurrency, query performance and migration risks. Use when schema, queries, transactions or data transitions warrant domain analysis; provide findings rather than a final PR merge verdict.
---

# Database review

Assess whether database-related changes preserve required data invariants and behave acceptably under relevant access patterns. Collect and interpret database evidence as part of the review; a full schema export or live database connection is not a prerequisite.

## Ground the investigation

Establish the reviewed revisions, affected data paths, database engine and relevant repository conventions from available code, migrations, schema, configuration and evidence. Trace callers and consumers as needed to understand business invariants, transaction boundaries, expected cardinality and competing writers.

Distinguish verified schema/runtime facts from assumptions about deployed state, engine version, data volume and traffic. Ask for or inspect further evidence only when it can materially affect the assessment. Use database tool skills such as `0-postgres` when applicable and useful; no connection or particular tool is mandatory.

## Exercise domain judgment

Choose the investigation, tools, depth and stopping point according to actual risks. These lenses are considerations, not required checks or an exhaustive list:

- **Integrity and semantics:** constraints, uniqueness, nullability, foreign keys, types, precision, time handling, defaults and whether existing data and all relevant writers preserve business invariants.
- **Transactions and concurrency:** atomicity, isolation, lock scope/order, lost updates, race conditions, retries, idempotency and partial failure. Evaluate the actual engine semantics rather than assuming an ORM operation is atomic or universally portable.
- **Queries and access patterns:** predicates, joins, pagination, ordering, index usefulness, N+1 behavior, round trips and bounded work. Connect performance findings to credible data size and access patterns; missing indexes or full scans alone do not establish a defect.
- **Migrations and data transitions:** compatibility with existing rows and readers/writers, DDL locking, table rewrites, constraint validation, backfill batching/restartability, partial completion and recoverability. Distinguish a migration existing in code from evidence that it ran successfully.
- **Data boundaries:** tenant scoping, authorization-sensitive predicates, parameterization, sensitive data handling and accidental broad updates/deletes.

Follow other consequential evidence as needed. Prefer minimal corrections over speculative schema redesign or unrelated tuning. Reuse reliable code-review findings and investigate database-specific consequences rather than repeating the entire implementation review.

Seek the smallest useful evidence to resolve uncertainty. Query plans, representative fixtures, engine documentation and existing checks can help where relevant. Apply version-specific facts only when supported; do not present guessed engine behavior as evidence. A plan without execution does not prove observed latency, and a small fixture does not establish production performance.

## Findings and handoff

Return reviewed scope/revisions, material invariants, evidence-backed findings and unresolved material questions. For each finding provide severity, exact query/migration/code location or affected object, concrete trigger, failure mechanism, impact, evidence and minimum practical fix or verification. Use P0 for critical failures, P1 for high-impact defects and P2 for material lower-severity issues; omit negligible or purely stylistic concerns.

Distinguish confirmed defects, sufficiently evidenced risks and unknowns. Explain whether a finding warrants blocking and why; the final PR merge recommendation belongs to `2-pr-review`. If no material issue is found, state the scope and confidence limitations that matter. Do not require a separate report for an internal handoff; persist useful durable results under shared artifact conventions.

Coordinate with `1-production-review` where database transitions affect rollout ordering, mixed application versions or recovery across services. Share evidence and ownership of the underlying risk rather than producing duplicate findings. Neither skill is a mandatory prerequisite for the other.

## Execution boundaries

Default to static/read-only review and reuse applicable evidence. Connecting to a database is not permission to mutate it. Do not execute migrations, backfills, DDL, data repair, load tests or consequential queries without applicable authorization. Read queries can expose sensitive data, acquire locks or impose load; use scoped evidence and account for those effects. `EXPLAIN ANALYZE` executes its statement and is not a harmless substitute for a plan-only inspection.

Do not modify implementation or introduce unit tests unless requested. Missing live access alone does not block an otherwise supported assessment; disclose only material uncertainties and the smallest useful next verification.
