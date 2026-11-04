# Storage conventions

Git tracks only explicitly allowlisted example templates; all actual memory data is ignored. Examples mirror the logical structure below using `.example.md` or `.example.yaml` filenames. Exclude these templates from knowledge retrieval. On a fresh installation, copy `projects.example.yaml` to local `projects.yaml` and replace illustrative values with confirmed repository mappings. Never overwrite an existing mapping during setup.

Create real files only when meaningful; the logical structure is:

```text
memories/
├── INDEX.md
├── projects.yaml
├── knowledge/
│   ├── global/INDEX.md
│   └── projects/<project-id>/
│       ├── INDEX.md
│       ├── architecture.md
│       ├── environments.md
│       ├── integrations.md
│       ├── team.md
│       ├── dispatch.md
│       ├── references.md
│       ├── decisions/<descriptive-topic>.md
│       └── learnings/<descriptive-finding>.md
└── working/<project-id>/
    ├── <task-id>.md
    ├── spec-queue.md
    ├── requests-inbox.md
    └── release-calendar.md
```

This is a convention, not a required rigid schema. Keep directories shallow; additional project-specific topic documents are fine. Indexes are optional navigation, never mandatory payloads.

## Repository identity

Inspect Git remotes in the actual repository, without printing credentials. Normalize SCP-style `git@host:owner/repo.git`, `ssh://git@host/owner/repo.git`, and HTTPS URLs to `host/owner/repo`: discard userinfo, query/fragment, trailing slash and `.git`; lowercase host, preserve path case. Preserve meaningful non-default ports. Resolve SSH host aliases only through explicit `remote_aliases` in projects.yaml or verified SSH configuration. Do not infer ownership from a local directory name. Do not persist credential-bearing remotes.

Prefer origin; use other remotes as evidence. Match exact canonical identities against `projects.<id>.repositories`. Conflicting project matches are ambiguous, including fork/upstream conflicts; do not choose silently. A project can list multiple repositories. For no match, missing Git, or uncertain aliases, ask for scope before project writes. Continue useful code work without memory; search only explicitly known relevant/global scope. Never fall back to dumping all projects. After scope is confirmed, add a narrow mapping under the write protocol. Optional explicit `local_paths` lists of absolute repository roots can identify repositories without remotes; treat these as machine-specific fallback and do not infer identity from basename.

Illustrative YAML only (not installed project knowledge):

```yaml
remote_aliases: {}
projects:
  example-project:
    repositories:
      - github.com/example/service-api
      - github.com/example/service-worker
```

## Metadata and lifecycle

Templates are illustrative, not evidence. Replace placeholders before saving real data. Use `type: fact` for environment/team facts and `type: reference` for locations; filenames identify the topic. Decisions and learnings use their corresponding types. Add fields only when they help retrieval or verification.

For mutable facts (team membership, hosts, routing, document locations), record a source pointer or attributed requester/message and the actual `updated_at`. For `verified` facts also record `verified_at` and what source/check established the claim. Dates must reflect actual writes/checks, never template dates. Unknown sources or unconfirmed claims remain `provisional`; do not fabricate evidence. Mixed-status facts in one document need per-entry attribution/status rather than a blanket `verified` label.

Verify important or changeable facts against current sources before acting on them. A timestamp alone does not prove validity. If verification is unavailable, retain the uncertainty. Mark contradicted or suspect facts `stale`, with reason and conflicting source. Use `superseded` with `superseded_by` only for a known replacement; retain decision context, acceptance evidence, rationale, and relevant conditions. Archive completed material only when useful and safe; preserve unresolved requests, pending replies, and references. An inbox item with completed work and a pending response remains outstanding for follow-up.

## Dual-axis status convention

- `evidence_status` describes reliability: `verified`, `provisional`, `stale`, or `superseded`.
- `task_status` describes operational progress: `backlog`, `in_progress`, `review` (awaiting a relevant check), or `done` (the record's outcome is complete). Use `blocked` or `cancelled` when needed, with a reason.

Do not require both fields on every record. A queue entry may still need attribution or evidence for its factual claims. Legacy knowledge `status` values matching the evidence states mean `evidence_status`; rename on the next relevant edit. If both fields conflict, reconcile using sources. Do not bulk-migrate unrelated records or interpret legacy evidence status as delivery progress.

Completion depends on record type:

| Record | Meaning of done | Useful evidence |
|---|---|---|
| Implementation/spec queue | Intended implementation and required checks completed; drafting a spec alone is insufficient | Spec/issue link, relevant artifacts and verification |
| Ad-hoc request | Requested analysis/work completed | Result link; track `response_status` separately as `pending` or `sent`, with response reference/date when sent |
| Release calendar | Recorded release scope completed in the target environment | Release/deployment record and outcome |
| Checkpoint | Requested task outcome completed | Artifacts and actual check results |

For specs drafted ahead of implementation, record readiness/approval in the linked spec or a concise note; keep implementation in `backlog` until started. Jira-managed work stays linked to Jira rather than becoming a second independent status queue.

Release entries distinguish requested dates from confirmed commitments. Record scope, environment, absolute date with numeric timezone, requester/source, and who confirmed a commitment with its source/date. Resolve ambiguous dates such as “Monday” from the original request's date/timezone; ask if unclear. A note is not a scheduled reminder or execution authorization.

## Topic templates

Copy only a relevant template; do not create empty files for every topic. These linked files are the maintained examples, excluded from normal knowledge retrieval:

| Topic | Template |
|---|---|
| Team roster | [team](../../../memories/knowledge/projects/example-project/team.example.md) |
| Environments and services | [environments](../../../memories/knowledge/projects/example-project/environments.example.md) |
| Jira and team routing | [dispatch](../../../memories/knowledge/projects/example-project/dispatch.example.md) |
| Postman and API documentation | [references](../../../memories/knowledge/projects/example-project/references.example.md) |
| Accepted decisions and rationale | [decision](../../../memories/knowledge/projects/example-project/decisions/event-transport.example.md) |
| Debug/tracing findings | [learning](../../../memories/knowledge/projects/example-project/learnings/connection-pool.example.md) |
| Deferred implementation | [spec queue](../../../memories/working/example-project/spec-queue.example.md) |
| Small requests and replies | [requests inbox](../../../memories/working/example-project/requests-inbox.example.md) |
| Release targets and commitments | [release calendar](../../../memories/working/example-project/release-calendar.example.md) |
| Cross-harness recovery | [checkpoint](../../../memories/working/example-project/resume-task.example.md) |

Checkpoints also support non-Git and multi-repository tasks: identify actual artifact/repository locations, omit unavailable Git metadata, and state verification limits. Save observations and authored technical artifacts, not hidden reasoning or transcripts. Keep specifications in their established repository/Confluence location when one exists and link them from memory.

## Artifact storage and references

Prefer the established repository/workspace or authoritative document location for complete artifacts. Use project-scoped `references.md` for selective pointers: canonical path/URI, purpose, and relevant decisions or review status. Project scope is established by its directory and repository mapping. Keep approval separate from `evidence_status`: verifying that a draft exists does not approve its content. Update existing entries and documents rather than generating version suffixes.

If repository/workspace storage is unavailable, use a descriptive Markdown file in `knowledge/projects/<project-id>/` after resolving project identity. Preserve the full deliverable there, report its actual location and any unmet requested destination, and use the same write protocol. This fallback needs no new directory schema. Do not copy complete artifacts into memory when they already have a canonical home. Recovery checkpoints should link them.

Actual memory is Git-ignored local data, without inherent backup, version history, or cross-machine synchronization. Saving prevents dependence on chat history, but does not protect against losing the local store. Existing authorized version control or backup arrangements remain separate; do not introduce automatic commits or synchronization.

## Concurrent writes

All cooperating harnesses must follow this protocol for Markdown and YAML, including mapping/index edits. No database or locking service is needed.

1. Read the target and retain its exact bytes or digest (or record absence). Prepare a narrow proposed edit preserving unrelated content. Review provenance, metadata syntax, and sensitive values.
2. Acquire the store-wide lock with atomic `mkdir <memory-root>/.memory-write-lock`. Create the root first if needed. If the directory exists, stop/report contention or retry later; never remove another writer's lock. An abandoned lock requires checking ownership/process state and user approval before removal. Keep ownership information non-sensitive and release only your own lock.
3. While holding the lock, re-read the target and compare to the original snapshot, including absence. If changed, do not overwrite: release the lock, rebase the edit on the latest text, and recheck; report semantic conflicts instead of choosing a winner. Repeat for each affected file. Search for an existing canonical topic again before creating a new record to avoid duplicate creations under different names.
4. Validate the proposed YAML/frontmatter with an available parser, ensure Markdown fences/sections remain intact, and review the diff. If no parser exists, use simple metadata and inspect it explicitly; never claim parser validation. Write a unique temporary file in the target's directory, flush/close it, then use an atomic rename/replace (e.g. `os.replace` or same-filesystem `mv`). Preserve appropriate file permissions. Recheck the target immediately before replacement to detect non-cooperating edits where feasible.
5. Verify the resulting file, clean up only your own temp file, and release your lock in a finally/trap path. Update navigation under the same lock when needed. Each file replacement is atomic, not a transaction across multiple files; write knowledge before optional indexes, and report partial completion.

Resolve all local harnesses to the same canonical memory root, including when code worktrees differ. Lock scope covers knowledge, working state, and projects.yaml on that filesystem. The lock directory and its contents are ignored by Git. Atomic rename prevents partial-file reads; the lock and snapshot check prevent cooperating writers on the same host from losing updates.

Non-cooperating editors can still race between the snapshot check and replacement; this is not guaranteed compare-and-swap. If tools cannot support locking and atomic replacement, defer the write and report the limitation rather than blindly overwriting.

### Multi-worktree and remote boundaries
- **Local filesystem only:** The `.memory-write-lock` is filesystem-local. It does not synchronize processes across different machines or isolated Git worktrees with detached storage.
- **Git synchronization:** Real memory is ignored by this repository; these instructions do not enable sync. If a separately authorized restricted store syncs memory via Git, agents must rely on standard Git branch/merge protocols and detect merge conflicts rather than assuming local locking provides global concurrency.
- **Sensitivity boundary:** Environment inventories and Vault path mappings should remain local or restricted. Never commit internal server topologies, infrastructure host/port mappings, or private Vault paths into public or widely-shared repositories.
