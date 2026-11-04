---
name: 1-requirement-analysis
description: Critically evaluate product requirements and produce actionable Requirement Findings for backlog refinement before technical design. Use for requirement clarity, product behavior, scope, and acceptance criteria review; not for technical specifications or overall backlog readiness decisions.
---

# Requirement Analysis

Evaluate whether requirements express the intended product outcomes clearly and coherently enough for further technical assessment. Improve decision quality through actionable Requirement Findings, without presuming either that requirements are sound or that they contain problems.

## Judgment and evidence

Choose the depth and direction of analysis based on the requirement, its consequences, and available evidence. Relevant dimensions may include business intent, expected outcomes, clarity and consistency, missing behavior, scope boundaries, assumptions, meaningful edge cases and failures, compatibility with existing behavior, and acceptance criteria that make outcomes verifiable. These are possible lenses, not a checklist or required sequence.

Challenge scope, assumptions, or proposed behavior when there is a meaningful reason. Evaluate product alternatives against the business context and their trade-offs. Complexity can be justified; simplicity, smaller scope, and reuse are not inherently better. Do not manufacture findings or alternatives to demonstrate critical thinking.

Use supplied context and relevant available sources, including PRDs, meeting notes, stakeholder decisions, business rules, repository code, dependencies, or related systems. Explore technical behavior and constraints when they affect requirement correctness; report their product implications rather than expanding into a technical specification. Existing behavior is evidence, not automatically the desired behavior.

Keep investigation proportionate to the decision at hand. Stop exploring when additional evidence is unlikely to materially change the findings.

Distinguish confirmed requirements, observed existing behavior, assumptions, proposed alternatives, and unresolved decisions. Ground material factual claims in inspected sources, citing locations where available. When sources conflict, surface the disagreement and any evidenced authority or superseding decision; do not silently choose a winner or invent approval. When evidence is insufficient, explain what remains uncertain and what information or decision would resolve it. Missing evidence alone does not prove a requirement is defective.

## Requirement Findings

Return concise findings suited to the size and significance of the request. Prioritize findings by their impact on product correctness, scope, and decisions, rather than by the number of issues discovered. For a material finding, convey the issue, why it matters, supporting evidence when available, and a useful clarification or alternative. Make unresolved decisions and dependencies visible where they affect the assessment. These are communication goals, not mandatory fields, categories, severity levels, or separate sections. Use no finding or question quotas.

When the assessed scope supports it, indicate whether the requirements are sufficiently clear for further technical assessment. Qualify conclusions by the scope reviewed and remaining uncertainty. This assesses requirement clarity only; it does not establish technical feasibility, engineering effort, or overall implementation readiness. If no material issues are found, say so directly. A small requirement may need only a brief assessment.

## Responsibility and independence

Work independently from the context available during invocation. Do not own grooming history or cross-session workflow state; saving the findings themselves remains this skill's responsibility. The calling agent or orchestrator owns context persistence, workflow coordination, unresolved-decision tracking, tool and companion-skill selection, and overall backlog readiness. Require no particular tool transport or other skill.

The deliverable is requirement evaluation. Do not automatically produce architecture, implementation plans, detailed technical designs, formal effort estimates, executable test cases, test execution, backlog prioritization, or final PO approval. These boundaries limit responsibility and output, not useful cross-domain reasoning.

## Saved findings

Save the Requirement Findings as Markdown when needed for approval, handoff, audit, follow-up, or long-term reference; keep trivial or temporary assessments in chat. Follow the shared artifact location precedence, updating the intended existing findings document on revisions. When persistence is needed, verify the saved file and return its actual path; report failed writes as unsaved rather than completed persistence. Saving the findings does not approve their recommendations.
