---
name: 0-codex
description: Execute Codex CLI headlessly and inspect its version and remaining usage. Defines execution, usage and version operations, permissions, models, effort and output capture; model routing belongs to 1-harness.
---

# Codex CLI

Own Codex execution mechanics and probes. The caller owns concurrency, reservations, integration and completion; `1-harness` advises routing/admission. Use [1-harness](../1-harness/SKILL.md) when routing is needed; no companion skill is required for a preselected operation.

## Three operations

These are skill operations using real CLI/API capabilities, not new MCP tools. Preselected operations do not invoke routing again. Reuse valid metadata; probe only when it can affect execution. Report capability support as `confirmed-supported`, `confirmed-unsupported` or `unverified`, with source/version: help confirms syntax, not end-to-end behavior.

| Operation | Input | Output |
| --- | --- | --- |
| `version` | optional executable, bounded timeout | available/missing/error, executable/version, observed_at, capability limitations; no inference of login/model access |
| `usage` | configured account/provider, optional pool/model, freshness need | known/partial/unknown/unavailable telemetry, source/time, pools/windows, remaining amount/percent/unit/reset and sharing; distinguish exhausted, stale and unknown |
| `execution` | cwd, prompt/file/context, owned scope, requested model/effort, authorized full/workspace/read-only mode, output/schema, timeout, run destination, existing run ID if reusable | process/task status, requested/effective settings where observable, enforcement/limitations, output/artifacts, changes/checks, optional usage/cost and partial effects |

Missing data is null with a reason. Session tokens/dollars are not remaining subscription quota. Unknown usage is not exhaustion; admission belongs to `1-harness`/caller. Never inspect unrelated credentials, change auth/billing, buy credits or enable overage for capacity.

Execute headlessly with an explicit cwd, unique run paths, literal prompt/stdin and bounded outer timeout. Honor existing authorization and controls; routing does not grant permission to enable full access. Full is not scope expansion; workspace/tool policy is not automatically filesystem isolation. Report prompt-enforced, tool-policy-enforced and actual sandbox coverage separately, including external tools/MCP/hooks/plugins. Unknown required enforcement blocks dependent execution.

Verify supplied model/effort support and preserve explicit pins; otherwise report configured defaults and observed identity where available. Effort names are provider-specific. Preserve an effective worker/context when possible; use explicit run IDs for resume, never an ambiguous latest session. Parse the actual output envelope, process exit and task status; sanitize logs and report meaningful checks.

Inspect partial edits/side effects before retry or handoff; preserve verified progress and don't replay mutations blindly. Diagnose environment, tool/permission, model capability and quota failures separately. Retry only with a changed condition/hypothesis and a task-appropriate bound; don't escalate model or permissions for every error. Caller owns rerouting, reservations and reconciliation; no nested agents unless explicitly assigned and budgeted.

## Version

Run `command -v codex`, `codex --version`, then relevant `codex --help` / `codex exec --help` for capability discovery. If missing, consult [installation](references/installation.md); do not install or run a model merely to probe availability. Verified locally on 2026-11-08: codex-cli 0.166.0. Recheck flags after upgrades.

## Usage

For ChatGPT-authenticated Codex, use the documented app-server protocol: start `codex app-server`, initialize the connection, send `initialized`, then request `account/rateLimits/read`. This is an account metadata probe, not a model turn. Read `rateLimitsByLimitId` when present; use the legacy `rateLimits` only as fallback, without double-counting. Convert `usedPercent` to `remainingPercent = 100 - usedPercent`; keep each window/reset and pool identifier. `model/list` on the same initialized connection discovers available model IDs and supported efforts; it does not prove a future model call will succeed.

Use a supported existing client and fresh metadata where available; no need to start a new probe for every task. See [protocol](references/probes.md) for the wire sequence. Bound the probe and shut down its child process. API-key/provider billing may not expose ChatGPT quota: report unknown/not applicable with billing mode, not unlimited. Do not redeem reset credits, change account state, or inspect auth files to fill missing quota. `codex exec` has no documented standalone `usage` subcommand in the verified local help; `/status` is interactive and must not be passed as a model prompt.

## Execution

Feed the complete prompt through stdin. Example for a preselected model/effort (replace placeholders with supported values):

```sh
codex -a never exec --cd /absolute/repo \
  --sandbox workspace-write --model MODEL \
  -c 'model_reasoning_effort="medium"' --ephemeral \
  --json --output-last-message /absolute/run/result.md \
  - < /absolute/run/prompt.md > /absolute/run/events.jsonl 2> /absolute/run/agent.log
```

- `full`: replace sandbox/approval settings with `--dangerously-bypass-approvals-and-sandbox`. This removes CLI approval/sandbox controls; external OS/managed controls still apply. Do not combine with `--approve-for-me`, which selects workspace-write automatic review.
- `workspace`: `--sandbox workspace-write` and global `-a never`; unavailable operations fail instead of waiting for input.
- `read-only`: `--sandbox read-only` and `-a never`; enforced sandbox for model-generated shell commands. Assess external/MCP tools separately if strict read-only is required; do not claim the sandbox constrains every remote tool.
- `--model` selects the model; `-c model_reasoning_effort=...` selects a supported effort. `--json` emits JSONL events, not a single final JSON object. `--output-schema /absolute/schema.json` constrains the final response; keep the final answer separate from events.
- Add `--skip-git-repo-check` only for a trusted non-Git working directory. Preserve repository instructions and existing user changes.

For continuable work, omit `--ephemeral` at creation and retain the run/session ID; use `codex exec resume --help` to verify ID-based continuation and its flags before resuming. An ephemeral run is not resumable; hand off its artifacts instead. Do not restart a capable run solely to rescore it.

## Sources

Local help is authoritative for installed flags. Official [CLI reference](https://learn.chatgpt.com/docs/cli/reference) and [app-server protocol](https://learn.chatgpt.com/docs/app-server), checked 2026-11-08. Version/help, initialized app-server quota and model/effort metadata probes executed successfully; no coding model execution was tested as part of this revision.
