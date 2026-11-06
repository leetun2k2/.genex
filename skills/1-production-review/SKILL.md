---
name: 1-production-review
description: Assess production risks of code or release changes, including compatibility, migrations, rollout, recovery and operational dependencies. Provide domain findings and verification gaps; final PR merge recommendations belong to 2-pr-review.
---

# Production review

Assess whether the proposed changes can coexist with the running system, be rolled out safely and recover from failure. Investigate relevant system consequences beyond changed lines. This is a reusable domain capability, usable standalone or within `2-pr-review`; it does not require a separate code review or full release plan.

## Establish the relevant context

Understand the change, reviewed revisions, affected components and available deployment context. Inspect consumers, schemas, configuration, manifests, dependencies and existing operational evidence where they affect the assessment. Distinguish documented intent, observed behavior and assumptions; challenge designs and earlier reports when evidence contradicts them.

Collect context as part of the investigation rather than requiring a complete input packet. Unknown production topology or rollout behavior is a material gap only when it can change the risk assessment; do not invent infrastructure or demand unrelated environment details.

## Apply production judgment

Choose depth, order, tools and stopping point from the actual changes and evidence. The following are useful lenses, not a checklist or exhaustive set of risks:

- Compatibility across deployed versions, clients, workers, events and shared data; whether mixed versions preserve essential contracts during rollout or rollback.
- Data and schema transitions: ordering, locks, backfills, partial completion, recoverability and whether old code remains usable after new writes or migrations.
- Runtime configuration, secrets references, dependencies and deployment ordering; whether the required settings and capabilities exist where the change will run.
- Failure behavior, retries, resource use and recovery across services; whether a local change can cause consequential system failures.
- Rollback or forward recovery, including state changes that reverting code cannot undo. A recovery path may be sufficient without a universal rollback requirement.
- Operational evidence needed to detect or diagnose consequential failures, where missing visibility materially affects safe operation.

Pursue other consequential risks when the evidence suggests them. Avoid repeating implementation defect analysis already supported by reliable code-review evidence. Follow affected call paths directly when needed to resolve a production question.

Seek the smallest useful evidence that resolves material uncertainty. Reuse applicable CI, tests and operational documentation; run safe existing checks only when useful and authorized. Missing optional checks do not establish a blocker, and green CI does not establish production safety. Keep static inspection distinct from runtime verification.

## Domain result

Return the reviewed scope/revisions, relevant invariants and a concise assessment of consequential risks and material verification gaps. For each finding give severity, affected location/component, concrete trigger and failure mechanism, production impact, evidence and minimum practical correction or verification. Distinguish confirmed defects, sufficiently evidenced risks and unresolved uncertainties. Use P0 for critical failures, P1 for high-impact issues and P2 for material lower-severity issues; omit cosmetic or negligible concerns.

Explain whether a finding warrants blocking and why, without owning the final PR verdict. If no material issue is found, state the scope and any limitations that materially affect confidence. No separate report or fixed output schema is required for an internal handoff. Persist useful standalone or durable findings under shared artifact conventions.

Default to read-only assessment. Do not modify implementation, execute migrations, deploy, alter infrastructure or mutate production merely to validate a hypothesis. Authorization for review does not authorize consequential operational actions.
