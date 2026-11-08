---
name: 1-harness
description: Recommend a harness, model and effort using task success, reasoning capability, continuity and productivity first, with lightweight cost/quota scoring and bounded admission. Reuse capable workers and route CLI operations to 0-antigravity, 0-claude or 0-codex.
---

# Harness routing

Recommend a **harness + model + effort**, including continuing an existing worker. Prioritize correctness, agent judgment and total completion time before resource savings. Caller owns authorization, decomposition, priorities, graph, concurrency, reservations, worker lifecycle and integration. This skill advises routing/admission; it is not another orchestrator.

## Reuse or fast path first

Route for a new worker or material change in task, capability, context, capacity or failure evidence. First consider a capable context-rich worker with verified progress. Reuse it when constraints/capacity permit; a small score advantage elsewhere doesn't justify switching. Continuation does not create another reservation.

For small clear work, choose the capable existing/configured route using valid metadata and a brief reason. No candidate enumeration, probes or score table merely to confirm an obvious choice. For consequential comparisons, use [decision support](references/routing-policy.md) with a few credible alternatives. Assess ambiguity, reasoning needs, verification difficulty and failure impact continuously, not by fixed task buckets. Favor sufficient end-to-end capability; neither cheapest-first nor largest-model-first is a default.

For multiple tasks, use caller-provided workload/priorities/dependencies to identify bottlenecks and scarce capable routes before assigning flexible easy work. Preserve capacity for identifiable hard tasks, not speculative demand. Don't create another graph, equalize provider usage or optimize each task greedily.

## Capability and constraints

Hard gates are authorization/pins, mandatory tools/modalities/context/isolation, supported model/effort, known unavailability and definitely insufficient quota/hard budget. Unknown required capability/enforcement must be resolved. Heuristic fit, estimated cost, reserve preferences and unknown quota are not hard gates.

Assess reasoning/coding together with harness tools, integrations, environment, context/workflow support, required skills/controls and relevant outcomes:

- **Capable:** sufficient task-specific support.
- **Uncertain:** suitability needs risk assessment/validation. Bounded low-impact verifiable work may provide evidence when authorized; critical work needs sufficient capability confidence first.
- **Incapable:** mandatory capability mismatch or relevant demonstrated inability; do not dispatch.

The Uncertain path cannot bypass unknown required tools/isolation. Do not label a model incapable from a low prior or identity alone. Evidence of actual mismatch matters; quota or price cannot compensate for unacceptable correctness risk.

Use [0-codex](../0-codex/SKILL.md), [0-claude](../0-claude/SKILL.md) or [0-antigravity](../0-antigravity/SKILL.md) for needed operations. Reuse valid version/capability metadata until change or contradictory behavior; refresh quota only when it could affect admission. Read current canonical execution instructions, but never invoke a model merely to probe availability. See [discovery](references/discovery.md) for stale catalogs.

A harness is not a model or pool. Compare actual tools/account/context, including native subagents; don't invent selectable settings or independent capacity. Distinguish documented capability, measured evidence and heuristic judgment. No unsupported provider strengths/prices/quota conversions.

## Recommend and admit

When comparison helps, use the reference's simple fit/productivity/resource scores. Scores support reasoning, not a mandatory ranking: override with a concrete correctness, complexity, risk or productivity reason. No heuristic quality floor excludes models. Among capable routes, stronger models need a meaningful reliability/latency/rework benefit; preserve useful context when switching overhead exceeds that benefit.

Apply reference admission to reuse and dispatch. Caller alone records/releases/reconciles shared-pool commitments and counts parent/native/nested work once. Unknown telemetry allows bounded execution with limited fan-out, not exhaustion or unlimited capacity. If all capable routes lack capacity, report `QUEUED` with a real wake-up/deadline or `BLOCKED` with a concrete constraint/next action; don't downgrade to Incapable or poll forever.

Pass task/context references, verified progress, owned scope/cwd, requested model/effort, existing authorized permission/enforcement, output/timeout and run destination. Never relax permissions to route faster. Preselected CLI operations don't call routing again; workers don't recursively reroute/spawn unless caller assigned and budgeted them.

Significant decisions record selected harness/model/effort, why capable/preferable, capacity effect and uncertainty/escalation condition (score/override when useful). Fast-path explanation can be one sentence. Caller retains actual checks/outcomes and commitments in its graph; selection is not task success.

## Recovery and learning

Inspect partial edits/side effects before retry/handoff. Diagnose environment, tools/permissions, reasoning capability and quota separately: fix allowed setup/context/strategy, adjust effort/model when justified, or use a permitted independent pool. Preserve explicit pins and authorization. Retry needs new evidence/condition and task-appropriate time/cost bounds; stop when progress stalls.

Keep the same effective worker unless switching gains justify transfer/integration/reverification. Handoff preserves context, verified progress and outstanding effects, never blindly replays mutations. Retain minimal useful task/context, actual route/effort, verified outcome, cause and known resource observations in the existing handoff. Reuse these for later decisions; no ranking database, global auto-edits or permanent capability verdict from one failure.
