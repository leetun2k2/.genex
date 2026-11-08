---
name: 2-development-review
description: Coordinate implementation review, functional test design and execution, and engineering acceptance against agreed requirements. Use after implementation or to re-review fixes; report defects and hand them back for flexible implementation without modifying application code.
---

# Development review

Own the **review** phase of `plan → implement → review`. Evaluate implemented work against agreed requirements using code and runtime evidence. Planning belongs to `2-development-plan`; implementation belongs to `2-development-implement`, which coordinates delegated coding and integration checkpoints. Do not modify application code during this phase.

## Establish the review basis

Read applicable instructions, requirements, accepted decisions, canonical planning artifacts, implementation diff/revision and existing verification evidence. Planning artifacts are helpful, not mandatory prerequisites; do not rerun planning merely because its skill was not invoked.

Identify reviewed scope, mandatory acceptance criteria and relevant contracts. Distinguish confirmed requirements from proposals and implementation behavior. Surface material conflicts rather than silently changing requirements or granting approval. Check whether prior reports concern the current implementation, test specification and environment before reusing their evidence.

Interpret the actual request: a static/code-review-only request does not authorize functional execution or environment mutation. A request for this full review phase includes applicable verification only within available permissions. Preserve existing authorization and ask only for missing decisions or permissions that affect the next action.

## Coordinate review and verification

Use current domain skill instructions through the available harness mechanism; require no fixed tool transport or subagents.

1. **Inspect implementation.** Use native code review to find material correctness defects, regressions and contract violations. Check relevant call paths and integration consequences. Reuse credible existing build/type/lint/test results or run appropriate existing checks when authorized and useful. Do not introduce new unit tests or unrelated cosmetic requirements.
2. **Ground the functional specification.** Reuse `1-test-design` artifacts where valid. Invoke that skill for missing or affected scenarios and implementation-dependent details, reconciling actual interfaces with agreed behavior. Preserve stable scenario IDs. Do not change expected outcomes merely to match the code.
3. **Execute functional scenarios.** Use `1-test-execution` for authorized checks, business assertions, side effects and cleanup. It owns execution evidence and PASS / FAIL / BLOCKED / NOT_RUN classifications. Keep execution reports separate from canonical test specifications.
4. **Assess fulfillment.** Use `1-acceptance-review` to synthesize requirements, code review findings and test evidence into READY / NOT_READY / INCONCLUSIVE. Its verdict is engineering acceptance, not PO approval or release permission.

Choose depth and ordering according to risk and dependencies. Static findings may make some execution unnecessary or unsafe; preserve what was not run and why. A narrower user request ends at its requested review boundary rather than automatically adding every activity above.

Each domain skill owns saving and verifying its deliverable. Save code review findings needed for remediation and a concise final handoff under existing artifact conventions. Reference canonical documents instead of duplicating them.

## Safety and human decisions

Before execution with side effects, establish target environment, permission, test-data isolation, affected services and cleanup. Non-production is not automatically safe. Do not deploy, run migrations or alter infrastructure merely to unblock verification without applicable authorization.

Ask only when affected work needs a material product/architecture decision, access or action outside current authorization. Explain the specific decision, why human input is needed, options, recommendation and what resumes. Reuse applicable approvals and continue independent safe review work. Missing runtime prerequisites must remain BLOCKED or NOT_RUN, never PASS.

## Defects and re-review

Route findings by ownership:

- **Implementation defect:** record expected versus actual behavior, evidence, impact and minimum remediation; hand back to `2-development-implement` outside this skill, with enough evidence to scope the correction and affected checks.
- **Stale/incorrect scenario:** reconcile through `1-test-design` against agreed requirements; preserve the reason for changes.
- **Architectural flaw or product ambiguity:** identify the affected planning artifact/decision and route to the relevant planning skill or human owner. Do not redesign silently during review.
- **Environment/access issue:** record the missing prerequisite and perform only authorized investigation or preparation.

If the surrounding request authorizes fixes, the caller may leave review, use `2-development-implement` for the scoped correction, and re-enter review without redundant permission. This is a phase transition, not permission for test execution or acceptance skills to edit code. If only review was requested, stop with findings and remediation handoff.

After fixes, inspect the new diff/revision and rerun only affected checks/scenarios plus justified regression coverage. Invalidate affected acceptance conclusions and evidence; preserve unrelated valid results. Require a concrete hypothesis or new evidence for retries. Stop cycles that reproduce a failure without learning and report the unresolved blocker.

## Durable result

Update the existing development handoff when available; otherwise save a concise review handoff near the feature artifacts. Preserve:

- Scope, requirements and accepted-decision references.
- Reviewed code revision, scenario/specification reference and environment.
- Code findings, actual repository checks and functional results with evidence links.
- Acceptance verdict and material limitations.
- Outstanding fixes, blocked prerequisites, pending human decisions and the next phase/action.

Verify saved artifacts and report their paths. On resume, compare code, requirements, contracts, scenarios and environment with prior evidence; repeat only affected work.

Separate implementation status, static review, repository checks, functional execution and acceptance in the final result. Never claim runtime verification from static inspection, or readiness from incomplete evidence. Do not commit, merge, deploy or grant formal business approval.
