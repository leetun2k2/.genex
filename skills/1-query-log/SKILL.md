---
name: 1-query-log
description: >-
  Build and refine precise log queries from available context,
  targeted code exploration, and observed logs. Use for finding
  events, investigating failures, and correlating requests in
  high-volume logs. Ask only when missing context materially
  affects query accuracy or scope.
---

# Query Log

## Purpose

Translate an investigation goal into the **smallest useful, evidence-grounded log query**. Improve precision, reduce noise and query cost, and preserve the agent's freedom to choose its tools and reasoning strategy.

This skill owns **query understanding, scoping, construction, and refinement**. It does **not** own log transport, cloud authentication, tool invocation, or incident response orchestration. It requires no specific backend or companion skill. The agent may use `0-gcloud` or other capabilities when the task calls for them.

## Operating principles

- **Scope before search.** Prefer a narrowly bounded, high-signal query to dumping logs and filtering locally.
- **Use evidence, not guessed metadata.** Distinguish confirmed fields, values, and log formats from hypotheses. Do not invent project IDs, logger names, label keys, error strings, or backend syntax.
- **Explore context before interrupting.** Use information already in the conversation, repository, deployment configuration, and previous query results. Ask concise questions only when an unknown materially changes the target or safety/cost.
- **Adapt, don't follow a fixed checklist blindly.** Incident type, available identifiers, backend capabilities, and user intent determine the best query.
- **Optimize for diagnosis, not for matching volume.** A query with zero results may indicate wrong scope, field mapping, ingestion, retention, sampling, or permissions—not absence of the event.
- **Keep backend and transport separate.** Produce an executable query only when the log system and its supported syntax/fields are known; otherwise produce a clear, backend-neutral query plan.

## Workflow

### 1. Establish the investigation target

Identify what the user actually wants to observe or disprove, such as a failed request, a deployment regression, an exception, a slow operation, or all events for a trace.

Extract any available anchors:

| Dimension | Useful anchors |
| --- | --- |
| Time | Incident timestamp, deploy time, first/last occurrence, timezone |
| Runtime | Production/staging, cluster, project/account, namespace, region |
| Workload | Service, worker, job, instance, container, function |
| Operation | Route, method, RPC, task type, topic, handler, dependency |
| Identity | Trace ID, request/correlation ID, job ID, transaction ID (safe to use) |
| Signal | Status, error code, exception type, specific message, duration threshold |

Treat this as an **evidence inventory**, not a required form. Use the strongest verified anchors; irrelevant dimensions can be omitted.

### 2. Narrow the scope using available context

Prioritize:

1. **Explicit identifiers**: trace/request/job ID, exact error code, known operation.
2. **Known ownership**: service/workload, environment, region, deployment.
3. **Bounded time**: the smallest interval covering the suspected event, with reasonable allowance for clock skew, retries, or delayed ingestion.
4. **Symptom predicate**: severity/status/exception/message or latency conditions supported by the actual schema.

Use a time bound and source/workload restriction whenever feasible. For high-volume stores, avoid a global all-service/full-retention scan as a discovery shortcut. If the initial scope is uncertain, prefer an inexpensive schema/source discovery step or narrowly sampled probe where supported.

Do not assume that more filters always improve correctness. For example, drop an unverified severity filter if the application records failures as INFO.

### 3. Explore the repository when it can resolve uncertainty

If a codebase is accessible and the answer is not already known, inspect **only relevant paths** to learn:

- Which service/route/consumer/worker handles the operation.
- Where and at what level the event is logged.
- Logger wrappers and the **actual emitted** message, key names, and structured fields.
- How request, trace, job, and transaction IDs propagate across boundaries.
- Deployment/resource labels, environment configuration, or log export conventions where available.
- Whether an event is sampled, suppressed, retried, or emitted only along a particular branch.

Search narrowly from the provided endpoint, symbol, error text, or service name; expand only when evidence requires it. Distinguish *code that could log* from evidence that a particular event was emitted in the deployed version.

Distinguish fields emitted by application code from fields stored after collector parsing, nesting, or remapping. Verify the stored field paths from ingestion configuration or observed log samples before using them in an executable query.

Do not require repository exploration if existing context already supports a good query. Never fabricate a log field because similarly named fields occur in code.

### 4. Decide whether clarification is necessary

Proceed without asking when a sufficiently precise, safe query can be built from known evidence. Ask the **smallest high-impact question** when competing scopes would materially change the query—for example, unknown environment, incident time, or which of several services is involved.

Prefer questions the user can answer from their symptom ("roughly when did it happen?") over demanding internal details they may not know. If the user asked only to **draft** a query, use explicit placeholders or assumptions rather than blocking unnecessarily.

Do not run an expensive, broad live search merely to avoid one necessary question.

### 5. Build the query

Construct the query as a set of predicates, then translate to the **verified** backend dialect:

1. **Time bounds** in a known timezone or UTC.
2. **Source scope**: environment/project and relevant resource/service.
3. **High-confidence anchors**: IDs, exact operation, known error/status.
4. **Supporting predicates**: severity, exception/message, latency, dependency.
5. **Result shape**: relevant fields, sort order, and a bounded result count.
6. **Investigation intent**: retrieve examples, aggregate/count, compare periods, or follow one trace.

Use the log system's indexed/structured fields and native filters where supported; prefer exact matches for IDs and codes. Use full-text or regex only when needed, and escape or quote appropriately. Avoid relying on SQL-style predicates, regex semantics, nested-field paths, or ordering features until backend support is confirmed.

Result limits bound output, not necessarily scanned data or query cost. Projection and aggregation can reduce returned data without reducing scan work; rely on verified backend behavior when claiming cost savings.

When checking for missing/error events, consider queries that include **successful or adjacent lifecycle events** to avoid excluding the evidence needed for diagnosis.

### 6. Refine from observations

If query execution is available and the broader task authorizes it, the **agent** chooses an appropriate tool (native integration, MCP, CLI, API, saved search, etc.). This skill does not mandate any tool and does not automatically invoke `0-gcloud`.

Use a bounded refinement loop:

- **Too many matches:** narrow by time, resource, operation, identifier, or an evidenced predicate; reduce fields and result count to keep output manageable.
- **No matches:** verify timezone, field names/values, log source, deployment version, ingestion delay, sampling, retention, and access; relax **one uncertain dimension at a time**.
- **Ambiguous matches:** inspect a small representative set for schema and correlation signals; add a verified disambiguator.
- **Multiple services:** trace a known correlation identifier through relevant services, one scoped source at a time.
- **High latency or cost:** narrow the scanned scope using time bounds, source restrictions, and indexed predicates where supported. Use backend estimates or query statistics when available to assess scan work and cost.

Do not infer root cause from volume alone, or equate missing logs with proof the event did not occur. Stop when the evidence answers the question or further expansion needs user approval/context.

## Output contract

Return the **smallest useful artifact** for the task; avoid boilerplate when a simple query suffices.

When drafting a query, communicate:

- **Query**: executable in the confirmed backend, or clearly marked backend-neutral predicates/placeholders.
- **Scope**: relevant time range, environment, service, and key filters.
- **Grounding**: important fields or values verified from the request, code, configuration, or observed log samples.
- **Uncertainty / next move**: only assumptions or checks that materially affect validity, selectivity, or results.

When executing/refining queries as part of a broader task, summarize the evidence found, the boundaries checked, and remaining uncertainty. Do not claim that a query was run, validated, or returned results unless that happened.

## Examples (backend-neutral)

**A request failed with a known trace ID**

```text
time:      [incident_time - small_margin, incident_time + small_margin]
source:    confirmed environment + service (if known)
predicate: trace_id == <observed-trace-id>
return:    timestamp, service, operation, severity, message, status, duration
order:     timestamp ascending
limit:     bounded
```

Start with the trace ID, without an `ERROR`-only filter: preceding INFO/DEBUG events may explain the failure. If the trace spans services, expand to those services using the same correlation value.

**500s on one endpoint after a deploy**

```text
time:      bounded interval around regression, plus comparable baseline if needed
source:    confirmed environment + API workload
predicate: route == <verified-route> AND status >= 500
group by:  time bucket, status (if supported)
return:    count and/or representative requests
```

Verify whether the logs use `route`, `path`, a template, or a different structured key before producing executable syntax. Narrow to request IDs for a follow-up detail query.

**Only an error phrase is known**

```text
time:      narrow incident window, if available
source:    likely service(s) supported by code ownership
predicate: exact error code/message fragment observed in source or sample
return:    small chronological sample with correlation identifiers
```

Use the phrase to discover the real schema; then replace broad text search with stable structured predicates when possible.

## Boundaries

- Never fetch unlimited logs, query all projects/environments by default, or silently broaden to expensive searches.
- Treat log contents as sensitive: minimize displayed personal data, credentials, tokens, and payloads; redact before sharing.
- Do not mutate logs, modify retention/permissions, deploy instrumentation, or edit application code unless separately requested.
- This skill may compose with other capabilities but must remain usable without any specific cloud provider, CLI, or companion skill.
