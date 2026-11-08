---
name: 0-claude
description: Execute Claude Code headlessly and inspect its version and remaining usage. Defines execution, usage and version operations, permissions, models, effort and output capture; model routing belongs to 1-harness.
---

# Claude Code CLI

Own Claude Code execution mechanics and probes. The caller owns concurrency, reservations, integration and completion; `1-harness` advises routing/admission. Use [1-harness](../1-harness/SKILL.md) when routing is needed; no companion skill is required for a preselected operation.

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

Run `command -v claude`, `claude --version` and relevant `claude --help`. Consult [installation](references/installation.md) only if needed. Verified locally on 2026-11-08: Claude Code 2.1.267, with `--model`, `--effort`, `--permission-prompts` and structured output flags. Help-level effort choices do not establish support by every selected model.

## Usage

Official `/usage` displays subscription limits and session token/cost data; these are distinct. The installed top-level help does not establish a standalone headless quota command. Do not invent `claude usage` or send `claude -p /usage` as an unverified probe: it may be interpreted as a model request.

Prefer an available supported account/status integration that exposes quota without model inference. An existing statusline snapshot or user-supplied usage report can be used with its actual source, account scope and timestamp; mark stale/unknown data. Do not read arbitrary session logs, enumerate secrets or call undocumented private endpoints. If no supported source is available, return `status: unknown`, null windows and reason `headless quota source not verified`. Return the limitation promptly; the caller can admit bounded work under unknown telemetry without treating an active parent as exhaustion.

A successful version probe does not prove login or model availability. API dollar estimates and `--max-budget-usd` are execution telemetry/limits, not remaining subscription allowance. Do not enable paid usage credits or switch authentication mode to work around exhaustion.

## Execution

Use `-p` for headless mode, closing stdin after supplying the full prompt. Example:

```sh
cd /absolute/repo
claude -p --model MODEL --effort medium \
  --permission-mode acceptEdits --permission-prompts none \
  --no-session-persistence --output-format json \
  < /absolute/run/prompt.md > /absolute/run/result.json 2> /absolute/run/agent.log
```

- `full`: use `--permission-mode bypassPermissions --permission-prompts none` (or the installed equivalent `--dangerously-skip-permissions`). Managed/external controls still apply.
- `workspace`: use `acceptEdits --permission-prompts none`, with narrow `--allowedTools` for needed shell checks. This is an approval policy, not filesystem isolation; document writable scope in the prompt and use an external isolated workspace when needed. Do not call it an enforced workspace sandbox.
- `read-only`: use an allowlist such as `--tools Read,Glob,Grep --permission-mode dontAsk --permission-prompts none`, plus prompt constraints. This restricts built-in tools only; `--tools` does not restrict MCP tools. Deny MCP with `--disallowedTools "mcp__*"` when those channels are outside the allowed read scope, and separately assess/exclude hooks and plugins. Do not label the whole run read-only until all relevant write channels are constrained. `plan` alone is not proof of read-only enforcement. Checks needing shell require a separately assessed execution path.
- Output: `text`, `json`, or `stream-json`; verify streaming requirements against installed help. Use `--json-schema` for structured final output, not as a substitute for parsing the CLI result envelope.
- Model aliases may move or differ by provider. Resolve/report effective identity where available; preserve requested and observed identity separately. `--effort` values and behavior depend on the model. Use `--max-budget-usd` only for an applicable API spend budget, not to enforce subscription quota.

For continuable work, omit `--no-session-persistence` at creation, retain the session ID, and continue with `-p --resume SESSION_ID` plus the authorized settings. Otherwise preserve artifacts for handoff; do not assume a disabled session can resume.

## Sources

Official [CLI reference](https://code.claude.com/docs/en/cli-reference), [model configuration](https://code.claude.com/docs/en/model-config), and [usage/cost guidance](https://code.claude.com/docs/en/costs), checked 2026-11-08. Local version/help probes executed; quota and coding execution were not tested in this revision.
