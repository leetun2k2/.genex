---
name: 0-gopls
description: Use gopls to navigate Go symbols, trace references and call relationships, inspect diagnostics, and make scoped renames, import fixes, and code edits.
---

# Go Language Tools

## Installation

If a required CLI or runtime is missing, read [installation and verification](references/installation.md).


Use the installed `gopls`. Choose commands based on the user's goal; check `gopls help <command>` for current syntax. Use an explicit subcommand: bare `gopls` starts a language server.

## Read and write policy

- Run in-scope read-only operations without user confirmation. Keep filters, limits, and timeouts as operational safeguards.
- Before any write, mutation, or configuration change, including local edits, prepare a concrete change set. Immediately before execution, show the target, exact commands with secrets redacted, and a diff or effects summary when no text diff is available. Ask for explicit user confirmation and wait for it. The initial task request alone does not satisfy this checkpoint.
- Confirmation covers the displayed change set, including repetitive sub-steps within it. Ask again only if the target, proposed changes, or side effects change.

## Inputs

Resolve these from the request and codebase. Ask only when a missing fact prevents useful work.

| Input | Details |
| --- | --- |
| Workspace | Repository/module directory and relevant `go.mod` or `go.work`; existing Go version, build tags, and environment. |
| Goal | What to understand, diagnose, or change; file, symbol, or diagnostic when relevant. |
| Edit scope | For edits: allowed files/symbols and desired change, such as a new name or import fix. Inspection alone does not authorize refactoring. |

## Usage

- Run from the relevant Go workspace. Reuse its toolchain and build settings; investigate package-loading errors before trusting missing results. Do not change modules, dependencies, or build settings merely to make a query succeed.
- Locate candidates with `rg`, `symbols`, or `workspace_symbol`, then inspect the source. Positions use `file.go:line:column` with 1-based lines and UTF-8 byte columns, or `file.go:#offset` with a zero-based byte offset. Recompute positions after edits.
- Choose `definition`, `references`, `implementation`, or `call_hierarchy` to answer the question. Read relevant callers and implementations before drawing conclusions. Results reflect the loaded workspace/build configuration; call hierarchies do not prove every runtime path.
- Use `check` on relevant files for diagnostics. Read the diagnostic output as well as the exit status; a clean result does not establish runtime correctness. Skip generated files by default, including `*.pb.go`, `*.validate.go`, and `*.gw.go`.
- Preview rename/import/format edits with `-diff` where supported. Show that diff and the intended `-write` command at the confirmation checkpoint before saving changes. Review the resulting diff and preserve unrelated user changes.
- List `codeaction` results before selecting one. `-exec` runs the first matching action; narrow by kind/title and inspect its effect. `-diff` controls edit output, not other command side effects; confirm any mutating action before `-exec`, even with `-diff`. Do not execute arbitrary custom commands or test-generation actions for routine navigation.
- Bound execution time with the runner and scope queries to the task. Use CLI subcommands for one-shot work; start MCP/server processes only when the workflow needs them.
- After edits, check affected files and review the diff. Report findings with file locations, diagnostics, and any workspace limits. Stop when the question is answered.

## Examples

Replace paths, symbols, and positions with inspected source locations. Run only the commands needed for the request.

```sh
# Find symbols and inspect a relevant file.
gopls workspace_symbol 'OrderService'
gopls symbols internal/order/service.go
gopls check internal/order/service.go

# Navigate the identifier at this exact source position.
gopls definition internal/order/service.go:42:18
gopls references -declaration internal/order/service.go:42:18
gopls implementation internal/order/service.go:42:18
gopls call_hierarchy internal/order/service.go:42:18

# List available fixes without executing them.
gopls codeaction -kind=quickfix internal/order/service.go

# Preview edits for a requested rename or import fix.
gopls rename -diff internal/order/service.go:42:18 FetchOrder
gopls imports -diff internal/order/service.go
```

For a requested rename, after showing its scope, command, and diff and receiving explicit user confirmation:

```sh
gopls rename -write internal/order/service.go:42:18 FetchOrder
gopls check internal/order/service.go
git diff
```

Example request: “Find callers of OrderService.GetOrder and its implementations. Explain the call path with file locations; do not change code.”

Reference: [gopls CLI documentation](https://go.dev/gopls/command-line).
