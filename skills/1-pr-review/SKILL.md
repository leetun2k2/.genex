---
name: 1-pr-review
description: Review a pull request for safe merging into a production-bound branch. Assess the complete PR and relevant system consequences, returning READY, BLOCKED, or NEEDS VERIFICATION. Use for PR merge safety decisions rather than general code review or feature acceptance.
---

# PR Review

Determine whether this PR is safe to merge into its target branch. Assume every PR may affect production. Apply production-grade strictness, agent-driven reasoning, and lightweight process.

## Scope and integration

Understand the PR identity, intent, target branch, head revision, and relevant system context. Review the complete effective PR diff from the target/head merge base to the PR head, not only the latest commit or local working changes. Distinguish the reviewed revisions from local edits. If the target or complete diff cannot be established, disclose the limitation and assess whether it prevents a responsible verdict.

Explore beyond changed lines as needed to understand consumers, dependencies, side effects, integration behavior, and invariants. Consider material interactions with the current target branch when it has advanced beyond the merge base.

Requirements, technical designs, task designs, and previous reviews are context, not unquestionable truth. Challenge assumptions in them and in the implementation. Separate intended behavior from observed behavior and unsupported claims.

This is a reusable workflow capability, usable standalone or within development workflows. `1-code-review` identifies material implementation defects; this skill owns the PR-level production merge recommendation, including integration, rollout, and verification sufficiency. `1-acceptance-review` evaluates requirement fulfillment and feature handoff readiness. Do not repeat either assessment wholesale or require prior invocation of either skill. Reuse reliable evidence while independently challenging gaps or contradictions that matter to this PR.

## Investigation and evidence

Choose the strategy, tools, depth, and verification according to the actual risk profile and repository context. Prefer native code review capabilities of the active harness when appropriate; Git, provider integrations, GitHub CLI, local exploration, static analysis, and relevant tests are alternatives or complements. No particular harness, transport, tool sequence, invocation count, number of passes, or test category is required.

Reason about system-level consequences. Relevant dimensions may include functional correctness and business invariants; backward compatibility and API contracts; data integrity, schema changes, and migrations; concurrency, idempotency, and distributed behavior; dependency and integration impact; security boundaries and sensitive information; configuration, deployment, and rollout compatibility; reliability, recovery, and rollback; and observability where operational diagnosis materially depends on it. These are considerations, not a checklist: investigate the dimensions justified by the changes.

Escalate investigation when evidence suggests consequential failures, a large blast radius, irreversible state changes, or uncertain integration assumptions. Seek the smallest useful evidence that resolves the material question. Stop expanding when the decision is adequately supported; do not demand exhaustive certainty or investigate every hypothetical failure.

Assess evidence for applicability to the reviewed revisions, environment, and failure mechanism. Passing CI does not prove production safety. Avoid redundant verification when existing evidence is sufficient; use relevant existing checks when they can materially improve confidence. State what was executed, what evidence was reused, and what remains unverified. Never imply runtime validation from static inspection alone.

## Findings

Report actionable, evidence-backed defects or credible production risks. Distinguish confirmed defects, sufficiently evidenced risks, and unresolved material uncertainties. Do not turn uncertainty into a confirmed defect or manufacture findings to appear thorough.

For each significant finding, communicate severity, exact file/line or affected component, concrete trigger and failure mechanism, production impact, supporting evidence, and the minimum practical correction or verification. Identify whether it blocks merging.

- **P0:** Critical production or security failure requiring immediate attention.
- **P1:** High-impact correctness, compatibility, data, security, or reliability issue.
- **P2:** Material production issue of lower severity; include only when its impact warrants action.

Prioritize P0/P1. Judge blocking status by consequence and evidence, not the severity label alone. Omit formatting, personal style preferences, speculative micro-optimizations, unrelated refactoring, and negligible hypothetical concerns. Prefer minimal corrections over redesign unless a material defect requires it.

## Merge decision

Choose one verdict for the reviewed revisions:

- **READY:** No identified blocking issue; sufficient relevant evidence supports merging. Material non-blocking limitations may remain.
- **BLOCKED:** A confirmed consequential defect or sufficiently evidenced unacceptable production risk prevents merging. Explain the remediation needed.
- **NEEDS VERIFICATION:** A material uncertainty prevents a responsible merge recommendation. Identify the specific missing evidence and the smallest useful verification needed.

An absence of findings is not sufficient for READY. Conversely, missing optional tests or speculative concerns alone do not justify withholding READY. If a known blocker and verification gaps coexist, use BLOCKED and identify the material gaps as well. Calibrate confidence to evidence; do not invent risk acceptance. A verdict is a recommendation, not PR approval or deployment authorization.

## Execution safety

Default to read-only review. Do not modify implementation, commit, publish review comments, approve, or merge without explicit authorization. Handle requested fixes as a separate authorized action. Before executing checks, account for side effects; use safe local or isolated verification and obtain authorization for external or consequential mutations. Do not create unit tests unless requested.

## Output

Keep the result concise and decision-oriented. Show the PR, target branch, reviewed revision or revisions, merge verdict with brief rationale, blocking or consequential findings ordered by severity, relevant verification evidence and limitations, and residual risks only when material. For BLOCKED or NEEDS VERIFICATION, make the next action clear. READY may need only a short paragraph when no material issues remain.

Follow shared artifact persistence and memory conventions. Save a review only when useful for approval, handoff, audit, follow-up, or durable reference; verify and link its actual path when saved. Do not require a separate report, duplicate global rules, or automatically commit or index the result.
