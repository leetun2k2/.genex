---
name: 1-task-design
description: Translate an existing technical solution into a cohesive Implementation Plan with task breakdown and task-level technical design. Use for implementation planning from a technical design or solution overview; not architecture authoring, standalone review, or coding the tasks.
---

# Task Design

Act as a Senior Software Engineer. Translate an existing technical solution into an **Implementation Plan — Task Breakdown and Task-Level Design** that a competent coding agent can implement using repository context and established conventions. Specify important engineering constraints while leaving routine coding decisions to the implementation agent.

## Engineering context and decisions

Use the supplied technical design or solution overview, requirements and constraints needed to interpret it, relevant repository context, and accepted decisions when available. No particular source format or prior skill invocation is required.

Inspect relevant code, call paths, interfaces, contracts, configuration, and existing mechanisms when they materially affect task correctness or boundaries. Support important factual claims with source locations when available. Distinguish verified behavior, confirmed requirements, accepted decisions, proposed local decisions, assumptions, and unresolved questions. Do not invent implementation facts, guarantees, measurements, or approvals.

Adapt investigation and detail to uncertainty and risk. Concurrency, persistence, distributed coordination, security, compatibility, and recovery may require deep investigation and precise constraints; routine changes may need little analysis. Stop when further evidence is unlikely to materially change the plan. Choose reasoning methods and tools freely, without fixed phases or investigation limits.

Treat accepted architectural decisions as constraints. Treat proposed or unresolved architectural decisions as conditional inputs, not approved implementation constraints. Resolve local engineering decisions needed for implementation, but do not silently change component boundaries, major data or communication strategies, system-wide invariants, or significant integration decisions. If investigation exposes contradictions, unworkable assumptions, missing major decisions, serious correctness risks, or significant scope changes, identify the architectural issue and recommend reconsideration with evidence. Do not force affected tasks through an invalid architecture. Keep these issues distinct from routine implementation questions.

## Cohesive implementation tasks

Decompose by implementable outcome and responsibility, not mechanically by file, function, class, interface, database method, or architectural layer. A task may span files and layers when they form one cohesive result. One task is sufficient when the solution is one cohesive change; do not manufacture work to enlarge the plan.

Identify dependencies and sequencing only where implementation or integration requires them. Keep independent tasks independent. Shared contracts or migration constraints may need brief cross-task guidance, but do not rewrite the entire architecture or produce a second system-wide detailed design.

For each task, communicate what changes, why it is needed, the implementation approach, and what determines completion. Include dependencies and risks that materially affect implementation. Useful detail may include expected behavior, relevant components, contracts and data changes, invariants, error handling, edge cases, integration constraints, and task-level verification expectations. These are communication goals, not mandatory fields or a fixed template; vary detail with the task.

Use precise interfaces, algorithms, transaction boundaries, ordering, idempotency, recovery behavior, or migration and rollback constraints where correctness or integration requires them. For example, a concurrency-sensitive task may need atomicity and stale-actor behavior specified, whereas a routine CRUD change may only need its behavior, relevant contract, and completion criteria. Distinguish intended guarantees and planned verification from demonstrated results.

Do not prescribe every function name, helper, file arrangement, code sequence, incidental abstraction, or design pattern. Repository references orient the implementer rather than becoming a mechanical file-edit checklist. Do not leave essential technical decisions unresolved in the name of autonomy. Avoid abstractions, services, configuration, infrastructure, refactoring, and premature extensibility without a concrete purpose; neither reuse nor minimal complexity is inherently the right answer.

## Readiness and deliverable

Ensure the plan accounts for all material outcomes, architectural decisions, and constraints from the source design. Identify omissions or unresolved coverage without manufacturing unnecessary tasks. Report mappings only when useful; no traceability matrix is required.

Make clear which tasks can proceed and which depend on unresolved decisions or evidence. A blocker in one task does not block independent tasks. Use safe, explicit assumptions where appropriate, explaining consequential implications if false; do not disguise high-risk unknowns as defaults or imply that proposed decisions are approved.

A task is sufficiently designed when a competent coding agent can implement it correctly using its guidance, repository context, and project conventions. The plan need not replace ordinary code exploration or specify every implementation detail. Include verification expectations proportional to the task's risk, using existing checks or meaningful scenarios where applicable rather than exhaustive test specifications.

Produce a concise, actionable plan with enough task-local detail to support correct implementation. Reference the source design for architectural context and shared decisions; keep unresolved architectural issues visible separately from local questions. Follow the user's artifact path or established repository conventions, preserving existing paths when updating a plan. Do not generate executable code or implement tasks unless separately requested.

Work independently and without hidden session state. The caller or orchestrator owns workflow coordination, context persistence, skill selection, approval tracking, and cross-skill handoff. Require no companion skill and do not invoke architectural design automatically. These responsibility boundaries constrain the deliverable, not technical reasoning across domains.

## Saved deliverable

Save the Implementation Plan as Markdown before completion, following the shared artifact location precedence; use a suitable workspace documentation directory when no convention exists. Read and update the intended existing document on revisions. Verify the saved file and return its actual path with unresolved decisions; if saving fails, report the deliverable as unsaved and incomplete.

Saving this deliverable belongs to this skill, even when invoked standalone. The caller's context persistence and handoff responsibilities do not replace it. A saved draft is not an approved design or authorization to implement.
