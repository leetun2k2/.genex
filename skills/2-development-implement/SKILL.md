---
name: 2-development-implement
description: Implement authorized development work by reading supplied documents, building a task graph with harness assignments and integration checkpoints, and delegating execution to subagents. Use for feature implementation or scoped fixes, not plan-only or review-only requests.
---

# Development implement

Own the **implement** phase of `plan → implement → review`. Coordinate subagents through a task graph and integrate their work at deliberate checkpoints. Each agent chooses its coding approach under repository instructions. Planning belongs to `2-development-plan`; independent verification and acceptance belong to `2-development-review`.

## Understand the complete input

Read all documents supplied by the user, including attachments and linked documents, before constructing the execution graph. Read long documents in parts rather than treating search excerpts or summaries as the whole input. Record inaccessible or incomplete inputs and their effect; do not claim they were read. Resolve material gaps before starting dependent work.

Inspect applicable repository instructions, relevant code, current working-tree changes and available planning/review artifacts. Preserve accepted decisions and distinguish them from proposals. Reuse a valid implementation plan; this phase adds execution dependencies, ownership and checkpoints rather than redesigning the solution. Formal planning artifacts are not prerequisites for a small authorized fix.

## Build the execution graph

Before launching workers, save a concise graph in the existing development handoff or adjacent implementation artifact. A table with dependency IDs is sufficient. For each task record:

- ID, intended outcome and work to perform.
- Scope: owned files or areas, shared contracts and excluded work.
- Dependencies and the integration checkpoint that consumes its output.
- Simple, observable completion criteria and relevant existing checks.
- Selected harness, execution workspace and eventual worker/run reference.

Keep completion criteria bounded to the task's outcome. Do not turn each task into a full acceptance review or relax agreed requirements merely to finish it. Split an oversized task instead of accumulating open-ended criteria.

Use [1-harness](../1-harness/SKILL.md) when selecting or materially reconsidering a worker/model/effort; reuse a capable existing worker and valid metadata where appropriate. Use its fast path for clear decisions and lightweight scoring only when comparison helps. Record the routing/admission reason and relevant uncertainty without inventing measured rankings. Supply task priorities/dependencies and remaining workload so routing can preserve scarce capacity for harder tasks without assigning work to incapable candidates. The coordinator owns shared-pool reservations, accounts for parent/native work once and reconciles results before further dispatch; unknown usage follows bounded admission rather than indefinite queueing. Load the selected `0-codex`, `0-claude` or `0-antigravity` for CLI execution; native workers use their actual exposed controls. Preserve pins and authorized permissions. Workers do not reroute recursively or create unbudgeted nested agents.

### Design concurrency around integration

Assess both write overlap and semantic dependencies: different files can still depend on the same changing interface. Establish shared contracts before dependent work or place their integration ahead of downstream tasks.

Prefer independent scopes. Accept small overlap only when its extent, integration owner and reconciliation approach are explicit. Never let concurrent agents overwrite the same live working files: serialize that portion or use isolated workspaces and reconcile at a checkpoint. Use separate worktrees or task directories when needed; a shared workspace is sufficient for disjoint ownership.

Insert integration checkpoints before divergence becomes expensive, especially after shared interfaces change or before several dependent tasks start. Each checkpoint identifies incoming tasks, its integration owner, the combined checks and the downstream work it releases. Favor short compatible batches over maximum parallelism. A small fix may have one worker and one checkpoint.

## Delegate and execute

Launch subagents for ready tasks using their assigned harnesses. Give each worker the outcome, document/context references, accepted contracts, dependency baseline, owned scope, completion criteria, checks and integration destination. Include applicable authorization limits. Request changed files or patch location, actual checks/results, assumptions and blockers in the return.

Workers may select coding techniques and tools within scope. Scope or contract changes that affect another worker return to the coordinator before that work proceeds. Keep one owner for shared integration files and the canonical graph; workers report updates rather than racing to edit it.

Track running tasks and results. Release a dependent task only after its prerequisites have been integrated and checked, not merely because a worker reports completion. Continue independent tasks when another is blocked. If delegation is unavailable, report that limitation instead of claiming subagent execution occurred.

## Integrate at checkpoints

Collect the incoming work, inspect actual diffs against assigned scopes, reconcile small conflicts and run the checkpoint's checks on the combined result. Integration may mean checking shared-workspace edits or applying patches from isolated workspaces; it does not require a Git commit or branch merge.

If overlap grows beyond the planned reconciliation, pause affected workers, integrate earlier or revise ownership/dependencies before continuing. Feed integration defects back as bounded corrective tasks. Preserve unrelated user changes. Do not repeatedly relaunch a failed task without a concrete hypothesis or new evidence.

A worker's success is provisional until integration. Record failed or unavailable checks explicitly and withhold dependent work that requires them; do not treat an empty result or successful process exit as evidence that task criteria were met.

## Resume and hand off

Update and verify the saved graph at checkpoints and before interruptions. Preserve task status, integrated baseline or uncommitted state, worker references, changed files, actual verification, blockers and next actions. On resume, compare current documents/code with that state and revise only affected tasks and evidence.

At the final checkpoint, verify the combined implementation against the graph's outcomes and run relevant existing repository checks. Do not introduce unit tests unless requested. Report completed and incomplete scope separately, with canonical artifact paths and verification limitations. Hand the resulting implementation and evidence to review; task completion does not grant engineering acceptance.

Use existing authorization across tasks and phase transitions. Ask only for material decisions or actions outside that scope. Do not commit, merge branches, deploy or execute migrations without applicable authorization. An implementation-only request does not automatically invoke full planning or review.
