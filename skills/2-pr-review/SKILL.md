---
name: 2-pr-review
description: Coordinate an evidence-backed PR review before merging toward production. Synthesize implementation defects, production risks and relevant verification into READY, BLOCKED or NEEDS VERIFICATION. Use for PR merge safety rather than feature acceptance alone.
---

# PR review

Own the recommendation on whether a particular PR is safe to merge into its target branch. Assume changes may affect production; apply production-grade judgment with proportional investigation.

## Preserve agent judgment

Use domain skills as capabilities and ownership boundaries, not mandatory stages. Choose the investigation, skill selection, order, depth, tools and stopping point according to the request, actual risks and available evidence. Combine work or reason directly when that is more effective. No fixed invocation graph, number of passes, agent count or test category is required.

Act as a reviewer who can investigate and decide, not merely a router or report aggregator. Follow consequential evidence beyond listed categories. Stop expanding when the decision is adequately supported; do not demand exhaustive certainty or manufacture findings.

## Ground the decision

Establish PR identity, intent, target branch and reviewed head. Review the complete effective PR diff from target/head merge base to head, distinguishing those revisions from local edits. Consider material interactions with the current target when it has advanced. If identity or complete scope cannot be established, disclose the limitation and judge whether it prevents a responsible recommendation.

Collect applicable instructions, affected call paths, relevant requirements/contracts and existing review or CI evidence as needed. Requirements, designs and earlier reviews are context to evaluate, not unquestionable truth. Missing planning artifacts do not require a planning cycle.

## Draw on domain capabilities

Use current domain instructions through the available harness mechanism where they add value:

- `1-code-review`: concrete implementation defects, regressions, contracts and relevant security, data, concurrency or reliability concerns.
- `1-database-review`: database-specific integrity, schema/query semantics, transactions, locking, access patterns and migration risks when the changes warrant deeper data-layer analysis.
- `1-production-review`: compatibility with the running system, migrations, configuration/deployment dependencies, rollout and recovery consequences.
- `1-test-design`: missing or affected functional scenarios when a concrete material question needs additional verification.
- `1-test-execution`: authorized execution of defined scenarios and evidence of business assertions and side effects.
- `1-acceptance-review`: requirement fulfillment when feature acceptance is actually part of the request. Its acceptance verdict is distinct from PR merge safety.

Provider tooling such as `0-github` can supply PR metadata, revisions, discussion and CI. Prefer native review capabilities and existing repository tooling when appropriate; no particular transport or delegation is required. `2-development-review` owns feature-review coordination; do not automatically invoke it or duplicate its work.

Share revision/scope references and reuse applicable evidence. Domain work should supply relevant criteria, evidence-backed findings, material uncertainties and the smallest next action, without requiring a separate document per skill. Select only the work justified by the PR; invoking all listed skills is not a completion criterion.

## Synthesize evidence

Resolve duplicate or conflicting findings against the underlying code and evidence. Investigate material gaps directly or through useful domain follow-up. Judge applicability to reviewed revisions, environment and failure mechanism. Distinguish executed checks, reused evidence, static conclusions and unverified behavior.

Passing CI is evidence, not proof of production safety. Run relevant existing checks when they materially improve confidence and are safe and authorized; avoid redundant verification. Missing optional tests alone do not justify withholding readiness. Do not introduce unit tests unless requested.

Report consequential findings with severity, exact location or affected component, concrete trigger/failure mechanism, impact, evidence and minimum practical correction or verification. Distinguish defects, sufficiently evidenced risks and unresolved uncertainties. Use P0/P1/P2 consistently with domain reviews; decide blocking status by consequence and evidence rather than the label alone. Exclude style preferences, unrelated refactoring and negligible hypothetical concerns.

## Merge recommendation

Choose one verdict for the reviewed revisions:

- **READY:** No identified blocker; sufficient relevant evidence supports merging. Material non-blocking limitations may remain.
- **BLOCKED:** A confirmed consequential defect or sufficiently evidenced unacceptable production risk prevents merging. Identify minimum remediation.
- **NEEDS VERIFICATION:** A material uncertainty prevents a responsible recommendation. Identify the missing evidence and smallest useful verification.

No findings alone is insufficient for READY. Conversely, speculative concerns or optional missing evidence are insufficient to block it. When a known blocker and verification gaps coexist, use BLOCKED and identify the gaps as well. Do not invent risk acceptance. The verdict is a recommendation, not PR approval, merge or deployment authorization.

## Re-review and boundaries

After fixes, compare the new head and relevant target changes with the reviewed revisions. Reassess fixes and affected paths; invalidate impacted conclusions and reuse unaffected evidence only while applicable. Retry verification when there is a concrete reason to expect new information, rather than cycling without learning.

Default to read-only review. Do not edit implementation, commit, publish comments, approve, merge or deploy without applicable authorization. Handle authorized fixes outside the review phase, then reassess the resulting revision. Before checks with side effects, account for environment, isolation and permissions; a review request does not authorize production mutations or consequential external actions.

## Result

Give a concise decision-oriented result: PR and target, reviewed revisions, verdict with rationale, consequential findings ordered by severity, relevant verification evidence/limitations and smallest next action when needed. Include residual risks only when material. A supported READY may need only a short paragraph.

Follow shared artifact conventions for durable approval, handoff, audit or follow-up results; update canonical reports rather than duplicating domain outputs. Verify and link saved paths. Do not require a separate report for every review or automatically commit or index results.
