---
name: 0-skill-improvement
description: >
  Review feedback on individual or combined global skills, global
  rules, harness instructions, and skill orchestration. Diagnose
  root causes and propose minimal, evidence-based improvements.
  Require explicit user approval before modifying effective skills,
  rules, or harness configuration; review proposals may be saved before approval.
---

# Skill Improvement

## Purpose

Improve reusable global skills, global rules, and their harness
integration using real execution feedback.

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
- Asks to review or improve existing global skills or rules.
- Reports failures involving harness instructions or orchestration.
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

## Phase 2 — Trace Relevant Components

Discover actual locations and source-of-truth files from available
configuration, references, and harness context. Resolve symlinks and
generated copies to their canonical sources; do not assume fixed paths.

Inspect only components relevant to the observed issue:

- Individual or combined skills, dependencies, and referenced material.
- Shared global rules, especially `rules/global.md` when present.
- Harness instructions such as `AGENTS.md`, `CLAUDE.md`, and equivalent
  native instruction locations for the harness involved.
- Relevant repository instructions and their scope.
- Reference loading, input/output contracts, sequencing, handoffs,
  and lifecycle control.

Trace which instructions were actually loaded and their effective scope
and precedence using available configuration or execution evidence.
File existence alone does not establish loading. If loading cannot be
verified, record the uncertainty. Respect higher-priority harness
instructions and existing instruction precedence.

Do not scan the entire system or audit the full library by default.
Expand discovery only when evidence points to additional components.

## Phase 3 — Root Cause Analysis

Distinguish the following causes; multiple may contribute:

- Skill defect: ambiguous, incomplete, incorrect, or overly restrictive
  domain instructions or missing decision criteria.
- Global rule defect: a shared behavioral principle is flawed or missing.
- Conflict between a skill and a global rule.
- Conflict between global rules or harness instructions.
- Skill/orchestrator integration defect: incompatible skills, incorrect
  sequencing, unclear ownership, or broken handoffs and lifecycle control.
- Incorrect rule loading or instruction precedence: required instructions
  are absent, incorrectly scoped, or applied at the wrong priority.
- Execution-only defect: valid, loaded instructions were not followed,
  including tool misuse or execution-specific reasoning failure.
- Missing task context: unavailable requirements, evidence, or expectations.
- Tool/environment issue.
- No confirmed defect.

Distinguish confirmed causes from hypotheses. Do not change instructions
to compensate for an unrelated defect unless the change has independent
value.

## Phase 4 — Evaluate Improvements

For each confirmed or plausible cause:

1. Identify the smallest effective change.
2. Determine the correct ownership layer:
   - Global rules: cross-cutting behavioral principles.
   - Skills: domain-specific procedures.
   - Orchestration: sequencing, handoffs, and lifecycle control.
   - Repository instructions: repository-specific conventions.
   - Task context or tool configuration, where appropriate.
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

Preserve existing behavior unless a change is justified. Keep reusable
skills separate from triggered workflows. Do not turn one-off feedback
into a global rule or duplicate instructions across layers unless needed.

### Global Rule Audit

When shared rules or harness instructions contribute to the issue:

- Identify the relevant instructions and evidence of their contribution.
- Check for ambiguity, duplication, contradictions, gaps, outdated guidance,
  excessive restrictions, or guidance too generic to help decisions.
- Verify effective loading and precedence as described in Phase 2.
- Consider adding, updating, relaxing, moving, or removing a rule;
  determine whether another ownership layer is more appropriate.
- Evaluate effects on unrelated skills and workflows.
- Prefer concise, generalizable principles over special-case constraints.

Never weaken essential safety or correctness invariants merely to satisfy
feedback.

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
- Target component and canonical source-of-truth file
- Current problematic instruction, if identifiable
- Proposed change
- Reasoning and supporting evidence
- Expected behavior improvement
- Potential impacts and regression risks
- Confidence and remaining uncertainty when evidence is incomplete

### Recommendation
Choose one:
- UPDATE_SKILL
- UPDATE_GLOBAL_RULE
- UPDATE_ORCHESTRATION
- UPDATE_MULTIPLE
- NO_CHANGE
- NEED_MORE_EVIDENCE

Identify the actual ownership layer even when no label names it directly;
use UPDATE_MULTIPLE for changes spanning layers.

### Approval
Save a clearly identified Markdown review proposal when proposing changes requiring approval. Follow the shared artifact location precedence, update the intended existing proposal on revisions, verify the saved file, and return its actual path. If saving fails, report it as unsaved. Mark proposed changes as awaiting approval unless approval has actually been given; saving is not approval.

Before modifying effective skills, rules, harness instructions/configuration, or their symlinks, obtain explicit approval of the proposed scope. Do not request approval again when the current conversation already authorizes that scope. Without approval, stop after saving and presenting the proposal and requesting approval. Proposal files and useful memory references may be saved without applying the proposed changes.

## Phase 6 — Implement (Approval Required)

Proceed only after the user explicitly approves the proposal.

Apply only approved changes.

If approval is partial, apply only the approved subset.

If the user requests changes to the proposal, revise the
proposal before implementation. If the user rejects it, make no changes.

When implementing:

1. Inspect the latest target file state and confirm its canonical source.
   Edit source-of-truth files, not generated or symlinked harness copies.
2. Preserve existing conventions and structure.
3. Apply minimal, focused modifications.
4. Avoid unrelated refactoring.
5. Keep skills reusable and stateless.
6. Maintain compatibility with Claude Code, Codex, and other supported
   harnesses without hardcoding tool transport.
7. Preserve explicit separation between reusable
   capabilities and triggered workflows.

Do not silently expand implementation scope. If implementation uncovers
a material new change, propose it and obtain additional approval before
applying it.

## Phase 7 — Verify Improvement

After modifications:

- Review the actual diff.
- Check syntax and instruction consistency.
- Check relevant skill/rule conflicts, dependencies, reference resolution,
  and effective loading paths, including affected harness integrations.
- Verify the original feedback scenario against the new
  instructions where feasible.
- Check a representative normal scenario that should continue working;
  when global rules change, include representative unaffected workflows.
- Check for new conflicts or excessive constraints.

Use lightweight verification appropriate to the change.

Do not make unsafe external calls merely to test a skill.

If the original scenario cannot be reproduced, report
verification limitations. Distinguish static instruction review from
actual behavioral evidence; a hypothetical walkthrough is not a real run.

Report:
- Files changed and concise diff summary
- Behavioral improvements intended
- Verification performed
- Remaining risks

Never claim an improvement is proven without evidence.

## Output Rules

During review:
- Be concise and specific.
- Prioritize actionable findings.
- Reference concrete skill, rule, or harness instructions when possible.
- Separate observed defects from inferred defects.
- Do not output unnecessary full skill rewrites.
- Without approval, stop after saving and presenting the proposal; do not apply proposed changes.

During implementation:
- Report what actually changed.
- Highlight significant tradeoffs.
- Do not claim success based solely on a clean diff.

## Fundamental Principles

Feedback is evidence, not automatically a specification.

A successful improvement must increase overall instruction quality,
not simply make the latest example pass.

Do not make the agent less intelligent by replacing judgment
with unnecessary procedural instructions.

Prefer no change over a weak or speculative improvement.

Keep the process lightweight and stateless: no databases, persistent
feedback logs, scoring systems, unnecessary subskills, or mandatory
full-library audits.

Never modify effective global skills, rules, harness instructions/configuration, or their symlinks without explicit approval of the proposed changes. Saving a separate review proposal or memory reference does not grant that approval.
