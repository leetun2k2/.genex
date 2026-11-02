---
project: example-project
task_id: sample-feature
task_status: in_progress
updated_at: "<actual write timestamp with timezone>"
repositories:
  - path: "<absolute repository/worktree path>"
    git_branch: "<observed branch; omit if unavailable>"
    git_head: "<observed commit; omit if unavailable>"
---
# Task checkpoint template

Template only. Include relevant fields; state unknowns and omit unavailable Git metadata. For multiple repositories, record each location and its state.

## Goal and constraints
- Requested outcome: <outcome>
- Constraints and authorization scope: <relevant user requirements>

## Decisions
- <Accepted decision, rationale, conditions, and source; link full spec/ADR if available>

## Verified progress
- <Completed work confirmed against current artifacts>

## Modified files and artifacts
- <Repository/file path, purpose, and related spec/issue/log pointers>
- <Untracked or external artifact locations needed for recovery>

## Verification
- <Command/check, repository or environment, actual result, and limits>
- <Checks not run and why; do not imply they passed>

## Blockers and remaining work
- <Current blockers, unresolved questions, and remaining tasks>

## Next action
- <Concrete next step after reconciling checkpoint with actual state>
