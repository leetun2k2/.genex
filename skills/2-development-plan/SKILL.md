---
name: 2-development-plan
description: Coordinate requirement analysis, technical design, task planning and functional test design into a durable implementation handoff. Use to plan or revise development work before implementation; do not implement code or execute tests.
---

# Development plan

Own the **plan** phase of `plan → implement → review`. Coordinate existing domain skills without duplicating their reasoning. Implementation belongs to `2-development-implement`, which coordinates a task graph, harness selection, subagent execution and integration checkpoints while leaving coding choices to the agents. The review phase belongs to `2-development-review`.

## Establish the planning scope

Read the work item, relevant repository instructions, current code and canonical artifacts. Reuse valid findings, designs and plans; investigate missing context before asking. Separate requirements, observed behavior, accepted decisions, proposals and assumptions.

Plan-only requests authorize planning, not code changes. If the surrounding user request also authorizes implementation, preserve that authorization in the handoff; completing this skill still ends the planning responsibility. The caller may continue with `2-development-implement` under that request without asking for redundant approval. A saved plan alone grants no implementation permission.

## Coordinate the work

Load each applicable skill's current instructions through the available harness mechanism. No specific transport or subagents are required.

| Skill | Planning responsibility |
| --- | --- |
| `1-requirement-analysis` | Clarify product outcomes, scope, behavior and acceptance criteria; preserve Requirement Findings for handoff |
| `1-technical-design` | Establish solution direction, boundaries, contracts, invariants and consequential trade-offs |
| `1-task-design` | Translate the solution into cohesive tasks, dependencies, constraints and completion criteria |
| `1-test-design` | Define functional scenarios, observable assertions, preconditions, side effects and cleanup for subsequent review |

Use the shortest path supported by the available evidence. Keep documentation proportional; do not manufacture tasks or architectural decisions. Draft test scenarios once expected behavior and key contracts are grounded. Leave implementation-dependent details explicitly unresolved rather than inventing interfaces; review can reconcile them later.

Each domain skill owns saving and verifying its canonical deliverable. Preserve existing artifact locations. A proposed design is a conditional input, not an accepted decision. Do not force affected tasks through unresolved architectural flaws; continue independent planning where useful.

## Human decisions

Move between planning activities without routine approval gates. Decide ordinary engineering choices within scope. Ask only for material product ambiguity, consequential decisions outside delegated authority, conflicts with accepted decisions, or scope/authorization changes that cannot be resolved from available evidence.

State the decision needed, why human input is required, options and trade-offs, recommendation, affected work and what will resume. Reuse applicable prior approvals; silence is not approval. Continue independent safe work while a dependent decision is pending.

## Implementation handoff and resume

Follow existing workspace artifact conventions. Keep one concise canonical development handoff near the feature documents when no convention exists. Record only what another agent needs:

- Goal, requirements/acceptance references, authorized scope and excluded actions.
- Repository/branch/revision inspected and links to canonical findings, design, tasks and test specification.
- Accepted decisions with their sources, proposals, consequential assumptions and unresolved decisions.
- Tasks ready for implementation, blocked dependencies and relevant existing verification commands.
- Test/environment prerequisites and the next action.

Do not copy complete domain documents or store secrets. Update and verify the handoff before HITL and at completion. On resume or requirement changes, reassess only affected decisions and artifacts; preserve valid work.

Finish by reporting whether planning is ready, conditional or blocked, with artifact paths and the concrete next action. Do not claim readiness where unresolved decisions prevent correct implementation.

## Phase boundary

Do not modify application code, run functional tests, commit, deploy or automatically invoke the review phase. Hand implementation to `2-development-implement`; it turns the planning handoff into an executable task graph and delegates implementation through the selected harnesses. The implementer chooses its approach under repository conventions and user authorization, and should carry forward actual changes, revision, checks and unresolved issues for review. No new unit tests are required by this skill.
