---
name: 1-api-contract
description: Write, review, or update concise, client-first API contracts in Markdown, saved inside the current workspace. Use for API handoff from requirements, technical design, or existing service code. Do not generate platform-specific specs or publish unless explicitly requested.
---

# API Contract

## Purpose

Produce a **client-consumable contract**, not an implementation design. A frontend, mobile, or service client should be able to integrate using the Markdown document **without reading backend source code or asking the author what an endpoint means**.

The contract is the authoritative **human-readable Markdown artifact** for this workflow. Do not automatically generate OpenAPI, Swagger UI, Postman collections, SDKs, or publish to any platform. Those are separate, explicitly requested activities.

## Guiding principles

1. **Client viewpoint:** Explain what the caller sends, receives, and does next. Describe observable behavior, not implementation.
2. **Minimal but sufficient:** Include only information that changes how a client integrates. One concise fact beats repeated explanations.
3. **Evidence before invention:** Use approved requirements/design, repository conventions, and actual service behavior. Never infer a field, status, or guarantee solely because it seems conventional.
4. **One consistent contract shape:** Organize HTTP APIs using OpenAPI's familiar concepts (operation, parameters, request body, responses), expressed in Markdown; apply HTTP semantics from RFC 9110. This is **not** a claim of OpenAPI compliance.
5. **Transport-neutral skill:** For non-HTTP APIs, retain the same client questions (operation, inputs, outputs, errors, semantics) and adapt names to the protocol; do not invent HTTP verbs/statuses for gRPC, events, etc.
6. **Workspace-local and platform-independent:** The document is a portable `.md` file; publishing and conversion remain the user's decision.

## Inputs and evidence

Accept any combination of requirement/PRD, technical specification, proposed API changes, existing source code, existing API contract, and client integration questions. The skill is **stateless**; gather context for the current task rather than relying on previous invocations.

Before writing:

- Identify the intended client and what it needs to accomplish.
- Determine whether the contract is **proposed** (design) or **observed** (current implementation). Approved spec/decisions govern proposed behavior; code and tests help verify observed behavior. Do not silently turn observed behavior into an approved design.
- Reuse existing conventions for authentication, naming, field formats, pagination, errors, status codes, and API versioning when verified.
- Inspect only relevant source/docs; avoid exploring unrelated systems or spending effort on internal architecture.
- If the user supplies only an API idea, draft a **proposed** contract with clearly identified unresolved decisions; do not represent it as implemented.

## Workflow

1. **Locate workspace.** Resolve repository root (e.g. `git rev-parse --show-toplevel`), otherwise use the current working directory. Respect an existing API-documentation folder if present; otherwise write to `docs/api-contracts/<feature-slug>.md`. Use a stable, human-readable feature slug. Do **not** save the resulting contract in the global skill directory, `~/.genex`, a temporary folder, or an external workspace by default.
2. **Collect the surface.** Identify only the client-facing operations relevant to the requested feature. Group them into one feature contract by default; split only when unrelated client workflows warrant it.
3. **Resolve essentials.** Determine method/path or protocol operation, auth, request fields/constraints, success response, actionable errors, and behavioral rules that can affect client integration. Cite or link existing shared conventions rather than duplicating them.
4. **Write the Markdown.** Start from `assets/contract-template.md`, then **delete every irrelevant section, instructional note, and empty placeholder**. Favor short tables and small valid request/response examples over prose. Document one normal success example per distinct payload shape; add an error example only if it conveys more than an error table.
5. **Review as a client.** Check the quality gate below. Inspect the diff for accidental breaking changes to an existing contract. Correct ambiguities you can resolve from evidence.
6. **Persist and report.** Create/update the Markdown in the workspace; return its relative path, a one-line coverage summary, and only genuinely blocking open decisions. Do not publish, change API implementation, or run generators automatically.

If workspace file-writing access is not available, provide the Markdown and the **proposed relative path** in the response; explicitly say it was not saved rather than claiming success.

## What every operation must answer

| Client question | Contract content |
| --- | --- |
| What can I do? | Short purpose + method/path (or protocol operation) |
| What access do I need? | Auth/scopes or link to shared auth rules; only if relevant |
| What do I send? | Path/query/header parameters, body schema, required fields, types, meaningful constraints/defaults |
| What do I receive? | Success status, content type/shape, field meanings needed to consume the result |
| What if it fails? | Known, actionable failure statuses/codes and what the client should do |
| What behavior affects integration? | Relevant pagination, ordering, state transitions, null/omission behavior, idempotency/retry, async handling, concurrency, rate limits, consistency or time format |

**Use exact wire names and types.** For fields where `null` differs from omission, say so explicitly. Clarify units, date/time representation, identifier format, enum values, and default/limit when they affect correct usage. For collections, specify pagination and termination behavior when applicable. For async operations, explain how clients know work has completed. For stateful operations, state allowed transitions and expected conflicts when relevant.

## HTTP / error conventions

- Prefer the project's existing conventions over introducing new ones. Follow standard HTTP method and status semantics (RFC 9110).
- Document the primary successful response and **relevant, observable, actionable** errors; do not list every possible 4xx/5xx defensively.
- Use the existing error envelope and stable machine-readable error code if available. Do not silently replace it with a new one.
- Where a new error representation must be proposed, RFC 9457 Problem Details (`application/problem+json`) is an option, **not a requirement**. If adopted, specify its stable `type` identifier and client-relevant extensions; do not instruct clients to parse human-readable `detail`.
- Distinguish authentication failure (401) and insufficient permission (403) when both exist; do not assert either without evidence. For retry, idempotency keys, throttling/`Retry-After`, or conflict conditions, document only verified/approved guarantees.
- Include **breaking-change implications** when updating an existing contract: renamed/removed/required fields, altered defaults or semantics, incompatible response/error shapes, behavior changes. Do not treat schema addition as automatically safe without considering clients.

## Document shape and brevity

The default Markdown contract contains:

1. **Title + state** (`Proposed`, `Review`, or `Approved` when explicitly confirmed) and one-sentence client purpose.
2. **Shared access/conventions** (brief; link existing docs if available).
3. **Endpoint index** when there are multiple operations.
4. **Operation blocks** with concise request, response, error, and critical behavior information.
5. **Open decisions** only when unresolved items can change how a client integrates.

Keep optional sections optional. Avoid: database entities, SQL, class diagrams, service topology, internal validation pipeline, implementation pseudocode, exhaustive QA cases, design debates, change logs, duplicated shared error tables, and large examples that teach nothing new. Never copy an entire internal DTO if only a subset is exposed on the wire.

Use simple Markdown tables and fenced JSON. Examples must be valid, representative, and consistent with the field contract. Avoid speculative timestamps, IDs, values, and status codes being mistaken for verified production behavior; label proposed examples appropriately.

## Uncertainty and handoff

- Resolve facts from provided material/relevant code first; ask the user only for **blocking** decisions that cannot be reasonably resolved.
- In the saved draft, mark each unresolved client-visible decision as `TBD` under **Open decisions**, with its impact and precise question. Do not scatter ambiguous guesses throughout examples.
- Clearly flag proposed behavior versus confirmed behavior. A contract with unresolved client-visible decisions cannot be marked `Approved`.
- For contradictions between approved design and current code, surface the mismatch in the handoff/report; do not silently choose a side or change the service.
- Never treat an example as a substitute for defining required/optional fields or behavioral guarantees.

## Quality gate (internal checklist)

Before reporting completion, confirm:

- A client can construct a valid call using this file and linked shared conventions.
- Every operation has an unambiguous success result and actionable known failures.
- Field names, requiredness, types, defaults, nullability, and constraints are accurate where material.
- Examples match the contract; no internal implementation details leak into it.
- There is no duplicated filler, invented guarantee, unresolved blocking detail mislabeled as approved, or accidental incompatible change.
- The artifact exists **inside the current workspace** at the reported relative path.

## Standards references (orientation, not required output)

- OpenAPI Specification: https://spec.openapis.org/oas/ — operation, parameter, request body, and response concepts.
- HTTP Semantics (RFC 9110): https://www.rfc-editor.org/rfc/rfc9110.html
- Problem Details for HTTP APIs (RFC 9457): https://www.rfc-editor.org/rfc/rfc9457.html
