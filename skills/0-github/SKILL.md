---
name: 0-github
description: Use gh to inspect and manage GitHub repositories, pull requests, issues, releases, and Actions runs, including CI debugging and authenticated API queries.
---

# GitHub

Use the installed `gh`. Choose commands based on the user's goal; use `gh <command> <subcommand> --help` when needed. Use `git` for local history, branches, and working-tree changes.

## Read and write policy

- Run in-scope read-only operations without user confirmation. Keep filters, limits, and timeouts as operational safeguards.
- Before any write, mutation, or configuration change, including local edits, prepare a concrete change set. Immediately before execution, show the target, exact commands with secrets redacted, and a diff or effects summary when no text diff is available. Ask for explicit user confirmation and wait for it. The initial task request alone does not satisfy this checkpoint.
- Confirmation covers the displayed change set, including repetitive sub-steps within it. Ask again only if the target, proposed changes, or side effects change.

## Inputs

Resolve these from the request, repository remotes, or existing CLI config. Ask only when a missing fact prevents useful work.

| Input | Details |
| --- | --- |
| Target | GitHub host and repository (`[HOST/]OWNER/REPO`), or a resource URL. Verify the intended repo when multiple remotes or forks exist. |
| Goal | What to inspect, debug, or change; PR/issue number, branch, tag, workflow, or run ID when relevant. |
| Change scope | For changes: the requested action, target, content, and options such as base/head branches or merge method. Investigation alone does not authorize publishing changes. |

## Usage

- Reuse existing authentication; check `gh auth status --hostname <host>` when needed. Keep tokens out of arguments, output, and files. Do not switch accounts, change scopes, or start login unless needed and requested.
- Prefer native subcommands; use `gh api` for missing fields or endpoints. Set `--repo` explicitly for repo commands when context is uncertain. For API calls, use the correct `--hostname` and an explicit repository endpoint; `gh api` has no `--repo` flag.
- Use `--json` with selected fields and `--jq` when useful. Bound lists with filters and `--limit`; paginate API results only as far as the task requires. Report when results are partial.
- For REST reads with `gh api`, specify `--method GET`: adding `-f` or `-F` otherwise changes the default method to POST. Inspect GraphQL operations for mutations separately.
- For CI debugging, inspect the relevant PR checks and run before fetching failed-job logs. Check exit status and distinguish failed/pending checks from authentication or network errors. Redact sensitive log data.
- Include merging, force pushes, deletion, workflow dispatch/reruns, and access or secret changes in the write confirmation checkpoint. Inspect API behavior and GraphQL operation bodies for side effects; the HTTP method alone does not establish a read.
- For multiline descriptions or comments, prepare the text in a file and pass `--body-file`. Before retrying a failed write, check whether it already succeeded to avoid duplicates. Verify the result and return its URL.
- Report findings with repository, PR/issue/run links and relevant evidence. State any access or coverage limits; stop when the question is answered.

## Examples

Replace sample targets and IDs. Run only the commands needed for the request; these examples use macOS zsh/bash.

```sh
github_repo='OWNER/REPO'

# List and inspect pull requests.
gh pr list --repo "$github_repo" --state open --limit 20 --json number,title,headRefName,url
gh pr view 123 --repo "$github_repo" --json title,body,baseRefName,headRefName,headRefOid,url
gh pr diff 123 --repo "$github_repo"
gh pr checks 123 --repo "$github_repo"

# Inspect issues and releases.
gh issue list --repo "$github_repo" --state open --limit 20 --json number,title,labels,url
gh issue view 456 --repo "$github_repo" --json title,body,comments,url
gh release list --repo "$github_repo" --limit 10

# Locate a run, then inspect only the relevant failed logs.
gh run list --repo "$github_repo" --branch feature/cache --limit 10
gh run view 123456 --repo "$github_repo" --log-failed

# Read one API page; use --hostname for GitHub Enterprise.
gh api --method GET "repos/$github_repo/pulls/123/files" -f per_page=30 \
  --jq '.[] | {filename, status, additions, deletions}'
```

For a requested draft PR, after preparing the branch, commits, and description file and receiving explicit user confirmation of the displayed PR creation command and effects:

```sh
gh pr create --repo "$github_repo" --draft --base main --head feature/cache \
  --title 'Fix cache expiry' --body-file /path/to/pr-body.md
```

Example request: “Inspect PR 123 in OWNER/REPO, explain its failing checks, and link the relevant run. Do not rerun CI.”

Reference: [GitHub CLI manual](https://cli.github.com/manual).
