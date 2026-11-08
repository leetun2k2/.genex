---
name: 0-antigravity
description: Execute Antigravity through the agy CLI headlessly and inspect its version and remaining usage. Defines execution, usage and version operations, permissions, models, effort and output capture; model routing belongs to 1-harness.
---

# Antigravity CLI

Skill name: `0-antigravity`; executable: `agy`. Own execution mechanics and probes; the caller owns routing, concurrency, budget reservations, integration and completion. Use [1-harness](../1-harness/SKILL.md) when routing is needed; no companion skill is required for a preselected operation.

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

Run `command -v agy`, `agy --version` and relevant `agy --help`. Consult [installation](references/installation.md) only when needed. Verified locally on 2026-11-08: agy 1.5.3. `agy models` lists model IDs; the local list contains Gemini and Claude families, so Antigravity is not synonymous with Gemini. Model list success does not prove a model execution will succeed.

## Usage

Run the CLI-handled metadata command separately from model execution:

```sh
agy -p /usage --print-timeout 20s
```

This path returned tab-separated quota rows in the verified local version: pool label, window label, remaining percent and reset timestamp. Preserve all windows, their units, source and observation time. Models can share a pool: do not treat every model as a new allowance. Preserve separately reported model-specific limits as additional constraints.

Do not put `/usage` into `--input-format stream-json`: it is a CLI-handled text report, not a normal model event. Do not force it through the execution JSON parser. If an installed version opens a TUI or does not handle the command headlessly, bound/terminate the probe and return unknown. Do not add `--disable-slash-commands`, which could turn it into a model prompt. Do not enable AI credits/overage to obtain capacity.

## Execution

Use an explicit model and effort supported together; discover IDs through `agy models`. Some IDs already encode effort; verify effective selection rather than combining contradictory ID and flag values. Example:

```sh
cd /absolute/repo
agy --print "$(< /absolute/run/prompt.md)" \
  --model MODEL --effort medium --mode accept-edits \
  --sandbox --dangerously-skip-permissions \
  --print-timeout 10m --output-format json \
  < /dev/null > /absolute/run/result.json 2> /absolute/run/agent.log
```

The quoted file expansion is a literal argument; do not construct the shell command by interpolating prompt contents. Add an outer timeout even with the CLI timeout.

- `full`: `--dangerously-skip-permissions`, omit `--sandbox` if full terminal access is required; external restrictions still apply. `--mode accept-edits` alone does not remove all permission prompts.
- `workspace`: `--mode accept-edits --sandbox --dangerously-skip-permissions` auto-approves requests and asks for terminal restrictions; it does not prove those restrictions are active. This is not proof of whole-process containment. Official reference says `--sandbox` can have no effect when unavailable; verify runtime coverage before relying on it. Never add skip-permissions merely because admission would be easier; this recipe requires existing authorization for that approval policy.
- `read-only`: enforced read-only support is unverified; local help establishes no such mode, which is not proof none can exist. State read-only constraints explicitly and return `enforcement: prompt-only`. For tasks requiring enforced isolation, mark Antigravity ineligible unless an adequate external sandbox/tool restriction is available. `--mode plan` and `--sandbox` do not themselves prove read-only behavior.
- Output: `text`, `json`, `stream-json`; `--json-schema` supports schema-controlled final output. JSON results contain task status, response and usage; streaming events have a distinct envelope. Parse actual task status, not just process exit.

For continuation, retain the conversation ID and use `--conversation ID` with the next `--print` prompt and existing authorized settings. Avoid `--continue` when concurrent runs make the latest conversation ambiguous. Preserve progress/artifacts if continuation is unavailable.

## Sources

Official [headless mode](https://antigravity.google/docs/cli/headless/), [quota command](https://antigravity.google/docs/cli/commands/usage/) and [CLI reference](https://antigravity.google/docs/cli/reference/), checked 2026-11-08. Local version/help, model discovery and a headless usage query executed successfully. No coding model run was tested.
