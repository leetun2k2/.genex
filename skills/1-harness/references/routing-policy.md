# Routing decision support

Revised 2026-11-08. Policy guides agent judgment; it is not a benchmark or a rule engine. Prefer task success and total completion time to nominal token savings. Distinguish **hard constraint**, **measured evidence**, **documented capability** and **heuristic**. No numeric quality floor or provider tier can establish or disprove task capability.

## Task fit and effort

Consider ambiguity, reasoning depth, integration/failure consequences, context already acquired, available tools and how cheaply errors can be detected. These vary continuously and may change during work. Assess the whole harness/model/effort: tools, integrations, execution environment, context/workflow support and applicable skills/permissions can make the same model capable in one route and unsuitable in another. Choose supported model/effort together; provider effort names are not equivalent. Stronger reasoning can reduce retries on hard work; low effort may suffice for focused, verifiable tasks. Increase effort only where useful; no automatic max or universal cheapest-first strategy.

The following are dated qualitative starting points, not cross-provider rankings or permanent model IDs:

| Models exposed by an eligible harness | Documented role to consider |
| --- | --- |
| OpenAI Luna | Focused, repeatable work |
| OpenAI Sol | Complex/repeated implementation and longer work |
| OpenAI Astra | Demanding reasoning across code and research |
| OpenAI Terra | Writing, visual understanding and supported app interaction; assess coding needs separately |
| Claude Haiku / Sonnet / Opus | Simple efficient work / daily coding / complex reasoning respectively |
| Gemini Flash / available reasoning models | Fast bounded work / deeper multi-step work; assess current model capabilities and tools |

Verify availability and relevant strengths before relying on these profiles. They do not prove any provider best at Go, UI, security or architecture. The same Opus model through Claude Code and Antigravity can have different context, tools, account/pool and cost. Favor local relevant outcomes over generic marketing; do not generalize one run into a global ranking. Unknown model fit calls for task reasoning, not an automatic penalty or rejection.

Sources checked 2026-11-08: [OpenAI model roles](https://learn.chatgpt.com/docs/models?surface=web), [Claude model roles](https://code.claude.com/docs/en/model-config), [Antigravity models](https://antigravity.google/docs/models/). These are qualitative guidance, not measured Genex performance. Discover current IDs/efforts only when needed; don't hardcode the dated local list as an allowlist.

Capability judgment is explicit: **Capable** has enough task-specific support; **Uncertain** needs proportionate validation/risk assessment; **Incapable** lacks a mandatory capability or has relevant evidence of inability. Unknown reasoning fit is not an automatic rejection, but critical work cannot be assigned on a hopeful price/quota trade-off. Low-impact verifiable work can serve as bounded validation when authorized. Mandatory capability/isolation unknowns remain blockers. Relevant execution results may change the judgment; model identity alone is not failure evidence.

## Fast path and numeric comparison

Reuse a capable worker or configured route for obvious choices. No score table, multi-candidate probes or metadata refresh is required when it won't change the decision. For meaningful comparisons, assess a few candidates on a coarse 1–5 scale (1 poor, 3 adequate/mixed, 5 strong):

- **F — task fit:** expected correctness, relevant reasoning/tools and ability to finish the whole task. Cite evidence or mark judgment. Once a candidate is capable, extra fit points require a task-relevant correctness/reliability benefit, not unused model power. A suspected capability mismatch needs an explanation; a low prior alone never excludes a candidate.
- **P — productivity:** expected completion time including probing/startup, inference, retries/rework, context transfer, integration and likely human intervention. Existing effective context matters. Qualitative estimates are sufficient; do not invent latency measurements.
- **R — resource suitability:** expected total execution cost and applicable quota pressure together. Use comparable billing/consumption evidence when available; otherwise explain a coarse estimate. Subscription allowance is neither free unlimited use nor interchangeable with unrelated API pricing. Include the opportunity cost of scarce quota needed by identifiable difficult pending work. Unknown telemetry is neutral (R=3, explicitly unknown), not exhaustion or an invented dollar estimate.

Default comparison: `score = 0.6*F + 0.3*P + 0.1*R` (1–5). These weights are adjustable heuristics, not mandatory optima. Change emphasis with an explicit task reason; uncertain numbers may be ranges. Do not add normalization formulas, fixed tie-break thresholds or numeric uncertainty deductions. Prefer a reasoned comparison over false precision. Scores cannot override hard constraints, prove correctness or justify quality/time loss for savings.

Keep an effective incumbent unless the expected improvement justifies handoff and revalidation. Select a lower score if concrete correctness, complexity, risk or productivity considerations warrant it; record the reason in a sentence. The caller need not approve routine routing choices already delegated.

Illustration only: an incumbent has F=4,P=5,R=3 → 4.2; a cheaper fresh route F=4,P=2,R=5 → 3.5. Reuse avoids transfer overhead. A difficult task may favor a stronger route with F=5,P=4,R=2 → 4.4 despite greater consumption. These are judgments, not observed timings or model benchmarks.

## Workload-aware choice without a new scheduler

Use the caller's task list, priorities and dependencies; do not invent speculative work or reorder execution outside its authority. Identify tasks with few capable routes and shared-pool bottlenecks before assigning flexible easy tasks. Recommend a capable economical route for those easy tasks when a stronger route offers no meaningful completion benefit. Preserve scarce powerful-model capacity for hard tasks that need it; allocate any reserve from this actual demand, not a fixed percentage. Reuse a context-rich worker when the transfer cost outweighs that saving. Never distribute evenly across unequal capabilities or sacrifice task correctness to fill a free pool.

Reserve/reassess assignments incrementally with the caller. If scarce capacity changes, propose alternatives **among capable routes**, or proportionate validation of uncertain routes where safe; never relabel an incapable route to force dispatch. If every capable candidate lacks capacity, return QUEUED with a real wake-up or BLOCKED with the limiting condition/next action. Scores remain decision support for overall workload completion, not an objective to maximize independently per task.

## Minimal quota admission

The caller owns a small admission record in the existing task graph: run IDs, shared pool/window scope, active status, commitments (units when known), observation time and next review point. Count parent, native and nested agents once by identity; continuing a run is not a new reservation. A parent is competing activity to account for, not an automatic lock on dispatch. Different models in a known shared pool do not create capacity; different harnesses don't prove independent pools.

Distinguish the following conditions; a stale prior exhaustion remains a warning until its reset/new evidence, not a reason to pretend quota is healthy:

| Condition | Decision path |
| --- | --- |
| Confirmed exhausted, definitely insufficient quota or hard spend budget | Do not dispatch there; use an eligible permitted independent pool or wait for a concrete reset/decision. Pins remain binding. |
| Known quota, credible same-unit task estimates | Fit commitments across every applicable window. Caller records each admission before the next; don't allocate the same allowance twice. A completion/review reserve may be prudent, but is adjustable policy, not a provider hard limit. |
| Quota or task consumption unknown | Permit one bounded additional worker per potentially shared pool by default within the caller's total concurrency/budget. Count existing additional workers, not the parent as consuming this child allowance; account for its activity and recent limit signals. Reuse an already-running capable worker first. No broad fan-out; more concurrency needs a concrete capacity reason, not an assumption of infinity. |
| Stale snapshot | Refresh once only if it changes admission and a supported inexpensive probe exists; otherwise follow the bounded unknown path. Never infer subscription allowance from session token counts. |
| Capacity temporarily reserved | Queue only behind identified live work with a completion/checkpoint/reset wake-up. Reassess at that event or a task-appropriate bounded deadline. |

Bound unknown-usage execution by one assigned task/slice, an outer timeout and the caller's existing cost/side-effect limits; reassess at its result/checkpoint or a limit error. A long indivisible task may use that single worker, with uncertainty disclosed. This does not guarantee remaining quota. Confirmed exhaustion/hard budget and authorization constraints still take precedence. Do not buy credits, enable overage, change auth or repeatedly probe to force capacity.

On queue wake-up/deadline, inspect current work once and choose: resume/reuse, bounded dispatch, permitted independent route, wait until a real known reset within the task horizon, or return an actionable capacity blocker. If no event or bounded progress path exists, do not keep QUEUED indefinitely. Lack of telemetry alone does not require human intervention; actual pins, hard limits or missing authority may.

## Reconcile without a second accounting system

Before resume/dispatch, the caller reconciles the graph against actual run state. Release completed/cancelled slots; don't expire a reservation merely by age while its worker may still run. Refresh provider data when useful, not per task; cache version until change and use quota timestamps/source plus known recent activity to judge freshness. No universal five-minute refresh mandate.

A fresh authoritative snapshot is the new baseline. Remove completed debits already reflected; for active tasks reserve only estimated **future** consumption, not their full original budget plus already observed consumption. If reflection or remaining estimates cannot be established, mark accounting uncertain and use bounded admission rather than accumulating stale deductions forever. Do not infer that a snapshot already includes pending charges. Outstanding known budget commitments and confirmed limits remain binding until reconciled. Preserve the assumption and next check rather than invent precise available slots.

On a rate-limit event stop new admissions to that affected pool; diagnose scope, respect the retry window and avoid sending every queued task to probe it. Reconsider routing after material task/permission/capacity changes, repeated poor progress, or loss of useful context—not because a minor score changed. The caller releases/reconciles reservations on success, failure, cancellation and interruption; routing only supplies recommendations. Workers receive bounded scope and no recursive/nested delegation unless explicitly budgeted.
