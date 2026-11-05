---
name: 2-development-orchestrator
description: Orchestrate an authorized development workflow from requirement analysis through implementation and functional test execution. Coordinate existing skills, maintain durable handoffs, resolve in-scope failures, and ask for human decisions only when genuinely required. Use for end-to-end implementation, continuation, or recovery of development work; respect review-only or plan-only requests.
---

# Development orchestrator

Coordinate work, not domain reasoning. Reuse the existing skills and native coding capability; do not duplicate their instructions or invent an orchestration framework.

## Outcome and boundary

Take an authorized work item from requirements to implementation and, where possible, functional test execution. Continue autonomously within the user's approved scope, including reasonable engineering decisions and fixing evidenced defects. Preserve approved decisions and the ability to resume after a human decision, interruption, or context compaction.

**Default end boundary:** test execution and an evidence-based final report. Do not silently extend to acceptance review, PR creation, merge, deployment, release, production operations, or broad unrelated refactoring. Those require separate user intent and authorization.

**Skill ownership:**

| Capability | Responsibility | Boundary |
| --- | --- | --- |
| `1-requirement-analysis` | Product behavior, ambiguity, scope and acceptance criteria; Requirement Findings | Does not approve implementation or overall readiness |
| `1-technical-design` | Solution overview, boundaries, contracts, invariants, trade-offs | Does not approve its own proposal or invoke task planning |
| `1-task-design` | Tasks, dependencies, implementation constraints and completion criteria | Does not implement or silently change accepted architecture |
| Native agent/harness coding capability | Implement authorized changes, review diff, run relevant existing repository checks | No separate implementation skill required; no new unit tests unless requested |
| `1-test-design` | Reusable functional/integration scenarios, chains, assertions, cleanup and coverage | Does not execute tests or produce executable scripts by default |
| `1-test-execution` | Execute authorized scenarios, inspect business/side-effect assertions, save evidence | Does not fix code or rewrite test expectations to make tests pass |

If an available skill or global instruction is more restrictive, honor it. Refer to each skill's current `SKILL.md` as the source of truth, not this summary. Invoke/load skills through the available harness mechanism; do not require a fixed CLI, MCP transport, API, or subagent.

## 1. Interpret the invocation and authorization

Determine intent from the user's actual request and applicable instructions:

- **Review-only:** inspect or critique; produce findings. No code changes or environment mutation.
- **Plan-only:** analyze/design/plan and save requested artifacts. No implementation merely because a plan exists.
- **Full flow:** explicitly authorized implementation or end-to-end development; analyze, design, plan, implement and verify within scope.
- **Resume:** continue previously authorized work from canonical artifacts and evidence; do not infer new permissions from previous progress.

A full-flow invocation needs an identifiable task and target workspace/repository. Find requirements, relevant existing artifacts, code conventions and checks from context/repository when available. Treat provided scope, access, constraints and environment as permissions **only to the extent explicitly granted**. Missing ordinary engineering details should be investigated rather than converted into questionnaires. If the user asks only to run the named skill without a work item, ask for the work item; if authorization to modify code is ambiguous, do not infer it from saved design documents.

A full-flow request can authorize local code changes in its stated scope; it does **not** automatically authorize external writes, irreversible actions, use of real customer data, migrations with side effects, deploys or writes to any non-production environment. Follow the applicable permissions and escalation requirements for each action.

## 2. Inspect and reuse before producing anything

1. Read applicable repository/global rules, selected skill instructions, task context and existing canonical artifacts.
2. Establish current reality: requirement status, accepted decisions, implementation revision, relevant tests, and evidence.
3. Validate that reusable artifacts still match current requirements, architecture and code. Reuse valid work; do not rerun every preceding stage for ceremony.
4. Build the shortest meaningful work path from the current state to the authorized outcome. Work on independent tasks while others are blocked, if safe.
5. Communicate the scope, important risks and immediate next action concisely. Do not create routine approval gates.

A stored artifact is **not** an approval. Distinguish accepted decisions, proposed decisions, assumptions and unresolved decisions.

## 3. Route work to the owning capability

The usual path is:

`requirements → technical design → task design → implementation → functional test execution`

Include `1-test-design` once the expected behavior is sufficiently grounded. Prefer drafting functional scenarios after requirements and key contracts are stable, potentially in parallel with implementation. Recheck and selectively update scenarios after actual interfaces, persistence and side effects are verified. Do not force a single strict ordering when dependencies permit better sequencing.

For each stage:

- Delegate reasoning and deliverable shape to the relevant current skill.
- Ensure each required canonical Markdown artifact is saved and verified according to its owning skill's instructions. Keep Requirement Findings available for downstream handoff in this workflow.
- Do not interpret a completed deliverable, recommendation, or proposed decision as user approval.
- Move to the next authorized stage when prerequisites are met; no automatic human gate between normal stages.
- Scale investigation and documentation to the change's risk/complexity without skipping necessary evidence.

**Implementation owner:** the coding agent/harness applies scoped edits, reviews the diff and runs existing, relevant build/type/lint/unit/integration checks as practical. Use native review capabilities when useful; do not create a mandatory extra code-review phase or add unit tests against global instructions. Do not claim checks passed unless actually run.

## 4. Human-in-the-loop policy

Ask only when the next dependent action needs a decision or authorization the agent cannot responsibly supply.

| Situation | Response |
| --- | --- |
| Routine technical choice consistent with accepted constraints | Decide and proceed; document only consequential trade-offs |
| Missing context discoverable in repo/artifacts | Investigate; do not ask prematurely |
| Safe, reversible assumption with low correctness risk | Proceed with explicitly stated assumption when useful |
| Material product ambiguity or acceptance behavior | Ask owner for decision before affected work |
| High-impact architectural choice, conflict with accepted architecture, or material scope change | Surface options and request approval before affected work |
| Write with nontrivial side effects, unsafe/unknown test environment, credentials, external services or deployment outside authorization | Stop the affected action and request the specific permission/dependency |
| Existing approval within scope and still applicable | Reuse it; do not ask again |
| Execution failure attributable to an in-scope implementation defect | Fix and selectively retest without a new approval |
| Repeated failure with no new evidence, or unresolved blocker beyond authority | Stop the affected loop; report blocker or ask for the required decision |

For each HITL request, state **decision needed**, **why the agent cannot decide**, **options/trade-offs**, **recommendation**, **affected work**, and **what will resume**. Batch related decisions where practical. Continue independent safe work; do not proceed on a dependent branch before the answer. Silence is not approval.

## 5. Handle failures by ownership and evidence

Treat failures as evidence, not a command to restart the entire workflow.

1. Have `1-test-execution` preserve observed evidence and classify each scenario `PASS`, `FAIL`, `BLOCKED` or `NOT_RUN`. HTTP success alone is not a functional pass.
2. Diagnose the most likely responsible layer:
   - In-scope code defect → native implementation; inspect diff, fix, rerun affected checks/scenarios and targeted regression.
   - Wrong or stale test expectation → `1-test-design`; reconcile with accepted product behavior, never quietly change assertions to produce PASS.
   - Technical contract or architectural flaw → `1-technical-design`; seek HITL only if the consequential decision is outside authority or conflicts with accepted architecture.
   - Product behavior or scope ambiguity → `1-requirement-analysis` and, if material, HITL.
   - Missing access/environment/data or unsafe side effect → BLOCKED; do only authorized preparation, request access/permission when necessary.
3. Require new evidence or a concrete hypothesis for another retry. If repeated cycles reproduce the same failure without learning, stop and report rather than loop indefinitely.
4. Invalidate and rerun **only affected** design/tasks/scenarios after changes; do not automatically rerun the full pipeline.

## 6. Execution safety

Before tests with writes/side effects, establish target environment, permission, test data/isolation, services affected, irreversible operations and cleanup expectations. Non-production is not automatically safe. Never use unapproved real data or production-adjacent effects to make the test run.

When execution is unavailable, preserve a useful implementation/check result and classify functional verification as `BLOCKED` or `NOT_RUN` with exact missing dependencies. Do not claim a simulated or unexecuted test is passed. Never silently deploy or manipulate infrastructure to unblock a test.

## 7. Minimal durable handoff and resume

Each domain skill owns the content and path of its own canonical deliverable. Avoid duplicated documents or copying full conversations into memory.

Follow any existing workspace convention for workflow handoffs. Otherwise keep **one small local Markdown state file**, e.g. `.genex/workflows/<work-item>/state.md`, containing only what a fresh session needs:

```md
# <work item>
- Goal / authorized scope / excluded actions:
- Repo / branch / revision:
- Mode / current stage / next action:
- Canonical artifacts: [requirements](...), [design](...), [tasks](...), [test spec](...), [test report](...)
- Decisions: accepted (with source), proposed, unresolved:
- Blockers / pending HITL decision:
- Implementation and verification: changed revision, checks executed, result/evidence:
```

Use the actual artifact locations produced by child skills; omit unused fields. Do not store secrets, tokens, raw personal data or bulky logs. Update the handoff at meaningful boundaries, before HITL, and when stopping or completing.

On resume, re-read the latest user instruction, repository state and canonical artifacts. Validate approved decisions and their applicability. Compare requirement/design/contract changes, code revision, scenario versions and test environment to verification evidence. Mark downstream outputs stale **only where affected**; redo the minimal necessary work. Preserve prior authorization only within its original scope.

## 8. Finish transparently

Final report should distinguish:

- **Implementation:** done / partial / not done; scoped changes and actual revision.
- **Repository checks:** what ran and its observed result; skipped checks and reason.
- **Functional execution:** PASS / FAIL / BLOCKED / NOT_RUN with evidence and environment, never conflated.
- **Outstanding decisions / risks / follow-up:** specific blocker, owner and next action.
- **Artifacts:** locations of canonical documents and resumable state.

Say **completed and functionally verified** only when authorized implementation and required functional scenarios have passed with credible evidence. An implementation may be complete while functional verification is blocked; report both statuses separately. Do not label the feature accepted by a PO or ready for release based solely on test execution.

## Operating principles

- One user invocation can advance through multiple skills; the orchestrator retains sequencing, context and authorization continuity.
- No required subagents, database, workflow engine, or artificial phase approvals.
- Favor the smallest complete solution, bounded scope and verified claims.
- Do not modify global skills/rules/harness configuration as a side effect of orchestrating one feature.
- Stop only the action that is actually blocked. Continue other safe, in-scope work when valuable.
