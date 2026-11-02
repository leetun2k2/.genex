# Jira commands

Checked with local `acli 1.3.39-stable --help`. Replace sample keys and IDs. Run only commands needed for the request.

## Read work items

```sh
acli jira project list --limit 20 --json
acli jira project view --key TEAM --json
acli jira workitem search \
  --jql 'project = TEAM AND statusCategory != Done ORDER BY updated DESC' \
  --fields 'key,summary,status,assignee' --limit 20 --json
acli jira workitem view TEAM-123 \
  --fields 'key,summary,status,assignee,description' --json
acli jira workitem comment list --key TEAM-123 --limit 20 --json
acli jira workitem link list --key TEAM-123 --json
```

- `view` takes a positional issue key; `edit`, `assign`, and `transition` use `--key`.
- `search` accepts `--jql` or `--filter`; `--count` counts matches and `--paginate` fetches all work items. Do not request all pages for a small sample.
- `view --fields` supports explicit field lists, `'*all'`, `'*navigable'`, and exclusions. Quote values containing `*`.
- `comment list --limit` is a per-page limit; its `--paginate` fetches all pages and ignores that limit. `project list --paginate` also ignores `--limit`.

## Create or change work items

Use the following only after showing the target, commands, and content diff or effects summary and receiving explicit user confirmation under the parent skill's write policy:

```sh
acli jira workitem create --project TEAM --type Task \
  --summary 'Fix cache expiry' --description-file /path/to/description.txt --json
acli jira workitem edit --key TEAM-123 \
  --description-file /path/to/description.txt --yes --json
acli jira workitem assign --key TEAM-123 --assignee '@me' --yes --json
acli jira workitem transition --key TEAM-123 --status 'In Progress' --yes --json
acli jira workitem comment create --key TEAM-123 \
  --body-file /path/to/comment.txt --json
```

Descriptions and comment creation accept plain text or Atlassian Document Format (ADF). For a JSON payload, use the leaf's `--generate-json` to learn its schema and `--from-json` to load it; do not guess the payload. These options are not universal.

- Resolve project-specific work item types and valid workflow status before changing them. A status name in an example does not prove a transition exists for the target.
- `create --label` and `edit --labels` have different names. Use `edit --remove-labels` for removal; do not infer append/replace behavior from the flag name. Read the current labels first and verify the result.
- Assignees accept email/account ID, `'@me'`, or `'default'`; removal uses `--remove-assignee` on edit/assign.
- `comment create --edit-last` changes the latest comment by the same author. Use only when that is the intended target. To edit a specific comment, use `comment update --key TEAM-123 --id 10001 --body-file /path/to/comment.txt`; ADF files use its separate `--body-adf` flag. Visibility and notifications require explicit intent.
- Verify updated fields with `workitem view`; inspect comments with `comment list`. Read the new key from the create result before follow-up actions.

## Links

Read supported types before creating links and verify direction; `--out` is the outward work item and `--in` is the inward work item. Help says `--type` accepts outward descriptions.

```sh
acli jira workitem link type --json
# After explicit user confirmation of the link command, target, type, and direction:
acli jira workitem link create --out TEAM-123 --in TEAM-456 --type Blocks --yes
acli jira workitem link list --key TEAM-123 --json
```

## Boards and sprints

```sh
acli jira board search --project TEAM --limit 20 --json
acli jira board list-sprints --id 123 --state active --limit 20 --json
acli jira sprint list-workitems --board 123 --sprint 456 \
  --fields 'key,summary,status' --limit 20 --json
```

Sprint work item listing requires both `--board` and `--sprint`. Its limit is per page. Prefer `board view` over deprecated `board get`, `filter view` over `filter get`, `filter list-columns` over `filter get-columns`, and `workitem list-watchers` over `workitem watcher list`. Check leaf help for their arguments.

For attachments, clone, archive, delete, bulk create, fields, dashboards, or filters, discover the relevant leaf through `--help`. Installed `workitem attachment` exposes `list` and `delete`, with no upload command shown; do not invent one.
