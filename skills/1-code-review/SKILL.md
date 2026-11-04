
---
name: 1-code-review
description: Review code changes for material correctness defects, breaking changes, regressions, and engineering risks. Prefer native harness code review capabilities. Focus on actionable, high-impact findings without overengineering.
---

# Code Review

## Purpose

Act as a pragmatic Senior Software Engineer.

Review implemented code to identify real defects and material risks, not to achieve theoretical perfection.

Optimize for high-signal findings, correctness, and minimal engineering overhead.

## Review Strategy

### Native-first

Prefer the current harness's native code review capabilities when available and appropriate.

- Use native review mechanisms, agents, or tools when they provide better review coverage.
- Adapt to the capabilities of Claude Code, Codex, or other harnesses.
- Fall back to direct code inspection when native review is unavailable or insufficient.
- Do not require a specific CLI, MCP, subagent, or review tool.
- Avoid duplicating equivalent reviews without a clear benefit.

### Context

Use available context:
- Changed code and affected call paths
- Requirements and acceptance criteria
- Technical design and task design
- Existing contracts, conventions, and behavior

Explore relevant repository context when necessary.

Do not assume that design documents are always complete or correct.

## Review Priorities

Prioritize:

1. Logic bugs and incorrect behavior.
2. Breaking changes and backward compatibility.
3. Regressions in existing functionality.
4. Violations of important requirements, contracts, or invariants.
5. Significant concurrency, persistence, reliability, or security defects.
6. Clear performance problems with meaningful impact.

Review proportionally to the scope and risk of the changes.

Do not perform exhaustive analysis of unrelated code.

## Avoid Overengineering

Do not report issues solely because:
- Code could be cleaner or more elegant.
- Another architecture or design pattern might be preferable.
- An abstraction could improve theoretical extensibility.
- A hypothetical edge case has negligible practical impact.
- A micro-optimization is possible.
- Existing code conventions differ from personal preferences.

Prefer established repository patterns and minimal fixes.

Do not recommend broad refactoring unless necessary to resolve a material defect.

## Finding Threshold

Report only findings with:
- A credible failure scenario or concrete contract violation.
- Meaningful impact on correctness, compatibility, security, reliability, or performance.
- Sufficient evidence from inspected code or relevant context.
- A practical, actionable fix or mitigation.

Ignore low-impact issues, stylistic concerns, speculative risks, and non-actionable suggestions.

Do not manufacture findings to fill the report.

When uncertain, investigate before reporting.

## Severity

- P0: Critical production or security failure.
- P1: High-impact defect or breaking change.
- P2: Meaningful defect worth fixing.
- P3: Minor issue; omit by default.

Prioritize P0/P1. Include P2 only when the impact justifies engineering effort.

## Output

Save the report as Markdown when needed for approval, handoff, audit, follow-up, or long-term reference; keep trivial or temporary assessments in chat. Follow the shared artifact location precedence, updating the intended existing report on revisions. When persistence is needed, verify the saved file and return its actual path; report failed writes as unsaved rather than completed persistence. Saving the report does not approve its recommendations.

For each finding:
- Severity and concise title
- File and line reference
- Evidence or credible failure scenario
- Impact
- Minimal suggested fix

Order findings by severity.

Keep the report concise and actionable.

If no material issues are found, state:

"No material issues found in the reviewed scope."

Mention significant unverified areas only when they affect confidence in the review.

Do not modify code unless explicitly requested.
Do not generate unit tests.
Do not claim runtime correctness without execution evidence.
