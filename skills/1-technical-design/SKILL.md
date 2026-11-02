---
name: 1-technical-design
description: Create or update complete, repository-grounded technical specifications from requirements. Use when the user asks to design a system, write a technical spec or RFC, or evaluate and document an architecture with an authoritative specification as the deliverable. Do not apply to document compression, standalone reviews or debates, debugging, or implementation requests without a design deliverable.
---

# Technical Design

## Purpose and output

Turn requirements and repository context into an **authoritative technical specification for implementation**. Optimize for design quality, not procedural compliance: correctness, technical completeness, and sound decisions are the goals. Define the **WHAT** (goals, observable behavior, contracts, invariants, boundaries), **WHY** (evidence, rationale, tradeoffs), and architectural **HOW** when it materially affects correctness or system behavior.

**Raw** means the complete written technical design, not private hidden chain-of-thought. Record decisions and supporting rationale; do not fabricate analysis or claim access to private model reasoning.

This skill produces raw specifications only. It runs independently of any briefing skill and does not automatically generate a brief or implement the design. Reconstruct context from the request, repository, and source files rather than hidden session state.

## Investigation and scope

Adapt investigation order, analysis methods, and design techniques to the problem; the following are quality expectations, not fixed phases.

1. Start with the smallest relevant evidence set: the requirement and relevant code, call paths, contracts, docs, or specs needed to establish current behavior, implicit invariants, compatibility constraints, and rollout risks. Do not assert existing behavior without evidence. Cite source paths, symbols, or contracts where they substantiate a material fact or decision.
2. Separate **known facts**, **assumptions**, and **unresolved questions**. Distinguish proposed behavior from verified current behavior. For consequential assumptions, state what changes **if false**.
3. Identify **in scope** and **out of scope** outcomes and their dependencies. Avoid inventing work to fill a template.
4. Separate required outcomes and explicit hard constraints from suggested solutions; treat suggested approaches as hypotheses. Challenge consequential assumptions with counterexamples, failure scenarios, and conditions that would invalidate the preferred approach. Compare material alternatives against existing mechanisms and the simplest viable solution; explain the choice under actual constraints. Preserve a proven mechanism, including recommending no architectural change, when added architecture offers no meaningful benefit. Scale comparison to the decision; trivial choices need no formal debate or alternatives table. Do not challenge explicit hard constraints merely to demonstrate independence.

Do not turn the spec into a questionnaire. Use explicit, safe, cheap-to-reverse assumptions where possible; reserve blockers for missing information with no safe default and high risk or substantial rework if chosen incorrectly.

Scale investigation depth to technical risk, not repository size. Expand when uncertainty could materially change correctness, architecture, compatibility, or implementation readiness; stop when further evidence is unlikely to change them. Reopen investigation when new evidence challenges an earlier conclusion. Use no fixed file, tool-call, token, or time limits. A short review target must never reduce the technical analysis or specification detail.

## Technical specification

Preserve the structure and decision logic required to implement the design correctly. When updating an existing spec, make the requested technical changes without unrelated restructuring. Do not reorder, compress, or remove technical detail for review speed. There are no arbitrary word, line, section, bullet, code-block, or diagram limits and no fixed presentation order or template.

Include the following when relevant to the change:

- **Architectural decisions:** Chosen approaches, boundaries, evidence, alternatives, consequences, and material tradeoffs. Make decisions affecting correctness, reliability, concurrency, security, compatibility, performance, and operations explicit.
- **Contracts and behavior:** Exact wire semantics, schemas, enums, errors, observable outcomes, invariants, acceptance criteria, and meaningful branch conditions. Retain complete definitions when needed for implementation or link precisely to canonical contracts.
- **Data and state:** Identity, ownership, relationships, persistence rules, lifecycle transitions, and data-integrity constraints.
- **Concurrency and recovery:** Required ordering, ownership/exclusion, atomicity, stale actors, retries, idempotency, partial failures, crash windows, and recovery paths.
- **Critical invariants and verification:** Identify the enforcing mechanism, conditions under which the guarantee holds, and realistic failure cases that could violate it. Specify verification proportional to risk, such as concurrency tests, fault injection, reproducible scenarios, contract checks, measurements, or operational evidence. Distinguish proposed mechanisms from demonstrated guarantees and planned verification from observed results. Design does not require implementing tests; include test code or implementation-specific test structures only when essential to the contract.
- **Security and compatibility:** Authorization and trust boundaries, sensitive data, existing callers and consumers, backward/forward compatibility, and coordinated version changes.
- **Migration and operations:** Schema changes, backfills, cache invalidation, deployment order, mixed-version behavior, rollback, resource limits, latency/capacity, and operational procedures or observability required for safe operation.
- **Risks and open decisions:** Consequential assumptions and their failure implications, blockers, unresolved questions, and items needing approval. Do not invent capacity figures, guarantees, or approval status.

Use source-backed domain names and examples. Include mandatory sequencing, failure handling, or implementation-level interfaces when required for correctness. Avoid incidental handler narration, generic best-practice filler, unnecessary boilerplate, and overengineering.

**Preserve implementation freedom:** Leave internal file layout, helper functions, and routine idiomatic code organization to the coding agent. Do not turn evidence references into a prescriptive file-edit checklist or leave essential architectural choices unspecified merely to avoid describing HOW.

Use diagrams when they improve technical understanding. One diagram should answer one question; split complex views into an overview and focused details. Use real domain terms. Diagrams are optional and must not replace precise contracts or invariants.

## Implementation readiness

For substantive or uncertain changes, classify each in-scope outcome:

| Status | Use when | Required detail |
|---|---|---|
| **Ready** | Requirements and inspected evidence suffice for safe implementation. | Observable outcome and relevant dependencies. |
| **Assumption — review** | An unclear detail has a safe default that is easy to change. | Proposed assumption, why it is safe, and what changes if false. |
| **Blocker** | Missing information has no safe default; a wrong choice creates high risk or substantial rework. | Missing decision or evidence, risk, and resolution needed. |

Do not mark an entire spec blocked when only one item is blocked. Ready items are independently implementable only if they do not depend on a blocker. Do not downgrade high-risk unknowns to make the design appear ready.

## Authority and updates

- The raw specification, including relevant linked raw documents and canonical contracts, is the **single source of truth for implementation**. A derived brief is never an implementation substitute; if they disagree, raw wins.
- Authority does not imply approval. Keep proposed decisions, accepted decisions, assumptions, and blockers distinct. Preserve accepted architectural decisions unless requirements change or new evidence invalidates their technical basis. Never silently replace them or adopt unaccepted review suggestions as settled design.
- Correct demonstrable factual errors and internal contradictions with supporting evidence; distinguish these corrections from architectural preference changes. If a correction requires reconsidering an accepted decision, flag the conflict and seek a decision before adopting a replacement.
- Record accepted changes and factual corrections in raw first. Preserve unresolved questions, consequential assumptions, blockers, and material risks until resolved. A changed raw document makes an existing brief potentially stale; report this when applicable without editing or automatically regenerating the brief.
- A ready or complete specification does not authorize implementation. Never auto-implement merely because the design is ready.

## File placement

Follow the user's requested path or established repository conventions. Otherwise:

- **Single feature:** `specs/<yyyy-mm-dd>-<feature>.raw.md`.
- **Independent flows:** Allow sibling `<yyyy-mm-dd>-<feature>-<flow>.raw.md` documents. Keep shared contracts and cross-flow invariants explicit and linked. Use a feature folder only when conventions or complexity warrant it.
- Use the current date for new default paths; preserve established paths when updating existing artifacts. Split by independent flow when useful, not mechanically by technical layer or to shorten a review.

Finish with links to all authoritative raw documents and identify outstanding blockers or approval needs.

## Before finishing

- **Completeness:** Could a coding agent implement the design correctly from raw and its canonical references? Preserve material decisions, exact contracts, state transitions, concurrency, failure/recovery cases, compatibility, and migration/rollout constraints.
- **Evidence:** Are current behavior, proposed behavior, assumptions, and unknowns distinguishable? Can material factual claims be traced to inspected sources? Are critical guarantees demonstrated or explicitly still unverified?
- **Readiness:** Are dependencies and blocked outcomes clear? Are safe assumptions justified and consequential unknowns still visible?
- **Freedom:** Are essential architectural choices explicit while incidental code organization stays open? Remove unnecessary prescriptions, not required technical detail.
- **Updates:** Were factual corrections evidence-backed and distinct from architectural decision changes? Were accepted decisions preserved or explicitly reconsidered, unresolved risks retained, and any affected existing brief flagged as potentially stale?
