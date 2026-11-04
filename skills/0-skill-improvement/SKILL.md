---
name: 0-skill-improvement
description: >
  Review user feedback after a skill or combination of skills
  produces an unsatisfactory result. Diagnose root causes,
  audit relevant global skills, and propose minimal,
  evidence-based improvements. Never modify skills without
  explicit user approval of the proposed changes.
---

# Skill Improvement

## Purpose

Continuously improve the quality of reusable skills using
real execution feedback.

Optimize for:
- Correctness and reliability
- Agent autonomy and reasoning flexibility
- Clear, maintainable instructions
- Minimal complexity and token overhead
- Compatibility with existing skills and workflows

Do not optimize merely for satisfying one isolated feedback
instance at the expense of general applicability.

## Trigger

Use when the user:
- Gives negative feedback after using skills.
- Asks to review or improve existing skills.
- Reports inconsistent output from a skill combination.
- Wants to prevent a recurring skill-related failure.

This is an explicit, user-triggered improvement workflow.

## Inputs

Collect available evidence from:

1. User feedback and expected outcome.
2. Original user request and relevant context.
3. Actual output and observed behavior.
4. Skills invoked or likely involved.
5. Relevant global rules and reusable instructions.
6. Orchestration and execution sequence, if applicable.

Prefer available conversation and repository evidence.

Do not require the user to reconstruct information already
accessible in the current execution context.

If essential evidence is unavailable, state the uncertainty
rather than inventing an execution history.

## Phase 1 — Understand the Feedback

Identify:

- What the user expected.
- What actually happened.
- The specific difference between them.
- Whether the issue is correctness, completeness, relevance,
  efficiency, formatting, consistency, or usability.
- Whether this is an isolated failure or a generalizable issue.

Separate:
- Explicit user requirements
- Reasonable expectations
- Subjective preferences
- Unsupported assumptions

Do not treat every preference as a global requirement.

## Phase 2 — Trace Relevant Skills

Discover the actual global skill locations and conventions.

Identify:

- Skills directly involved in the task.
- Dependencies and referenced rules.
- Related skills that may influence the behavior.
- Conflicting or duplicated instructions.
- Input/output contracts and handoff assumptions.

Inspect relevant skill content and references.

Do not audit the entire global skill library by default.

Expand the investigation only when the evidence indicates
a cross-skill or shared-rule problem.

## Phase 3 — Root Cause Analysis

Classify the failure:

A. Skill instruction defect
   - Ambiguous or incomplete instructions
   - Incorrect assumptions
   - Overly restrictive rules
   - Missing decision criteria

B. Cross-skill integration defect
   - Conflicting instructions
   - Incorrect sequencing
   - Ambiguous ownership
   - Broken input/output handoff

C. Execution defect
   - Agent failed to follow valid instructions
   - Incorrect tool usage
   - Incomplete context exploration
   - Execution-specific reasoning failure

D. Context or input defect
   - Missing requirements
   - Inadequate evidence
   - Conflicting expectations

E. Environment or tooling limitation

F. No confirmed defect

Distinguish confirmed causes from hypotheses.

Do not modify skill instructions to compensate for an
unrelated defect unless the change has independent value.

## Phase 4 — Evaluate Improvements

For each confirmed or plausible skill-related cause:

1. Identify the smallest effective change.
2. Determine the correct ownership layer:
   - Global rule
   - Individual skill
   - Skill combination / orchestration
   - Task-specific instructions
   - Tool configuration
3. Evaluate positive and negative impact.
4. Check compatibility with existing workflows.
5. Check whether the change creates duplication.
6. Check whether it unnecessarily restricts agent autonomy.
7. Consider whether no change is preferable.

Prefer:
- Generalizable decision principles
- Clear boundaries and responsibilities
- Explicit invariants
- Concise guidance
- Existing native harness capabilities

Avoid:
- Hardcoding individual examples as universal rules
- Excessive mandatory steps
- Duplicated instructions across skills
- Unnecessary templates
- Prescribing tools when adaptive selection is sufficient
- Expanding scope without evidence

Preserve existing skill behavior unless a change is justified.

## Phase 5 — Improvement Proposal

Present a concise proposal containing:

### Feedback Summary
Expected behavior versus observed behavior.

### Root Cause
Confirmed findings, relevant evidence, and uncertainty.

### Affected Components
Specific skills, rules, or orchestration boundaries.

### Proposed Changes
For each change:
- Target file or component
- Current problem
- Proposed adjustment
- Why it addresses the problem
- Potential regression risk

### Recommendation
Choose one:
- IMPROVE
- NO CHANGE
- NEED MORE EVIDENCE

### Approval
Explicitly request approval before editing.

If changes are proposed, stop here.

Do not edit files, create symlinks, modify global rules,
or perform other persistent mutations during review.

## Phase 6 — Implement (Approval Required)

Proceed only after the user explicitly approves the proposal.

Apply only approved changes.

If approval is partial, apply only the approved subset.

If the user requests changes to the proposal, revise the
proposal before implementation.

When implementing:

1. Inspect the latest target file state.
2. Preserve existing conventions and structure.
3. Apply minimal, focused modifications.
4. Avoid unrelated refactoring.
5. Keep skills reusable and stateless.
6. Maintain compatibility with supported harnesses.
7. Preserve explicit separation between reusable
   capabilities and triggered workflows.

Do not silently expand implementation scope.

## Phase 7 — Verify Improvement

After modifications:

- Review the actual diff.
- Check syntax and instruction consistency.
- Check relevant dependencies and references.
- Verify the original feedback scenario against the new
  instructions where feasible.
- Consider at least one normal scenario that should
  continue working.
- Check for new conflicts or excessive constraints.

Use lightweight verification appropriate to the change.

Do not make unsafe external calls merely to test a skill.

If the original scenario cannot be reproduced, report
verification limitations.

Report:
- Files changed
- Behavioral improvements intended
- Verification performed
- Remaining risks

Never claim an improvement is proven without evidence.

## Output Rules

During review:
- Be concise and specific.
- Prioritize actionable findings.
- Reference concrete skill instructions when possible.
- Separate observed defects from inferred defects.
- Do not output unnecessary full skill rewrites.
- Stop after proposing changes.

During implementation:
- Report what actually changed.
- Highlight significant tradeoffs.
- Do not claim success based solely on a clean diff.

## Fundamental Principles

Feedback is evidence, not automatically a specification.

A successful improvement must increase overall skill quality,
not simply make the latest example pass.

Do not make the agent less intelligent by replacing judgment
with unnecessary procedural instructions.

Prefer no change over a weak or speculative improvement.

Never modify global skills without explicit approval.
