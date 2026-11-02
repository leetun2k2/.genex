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
│       ├── decisions/<descriptive-topic>.md
│       └── learnings/<descriptive-finding>.md
└── working/<project-id>/<task-id>.md
```

This is a convention, not a required schema. Keep directories shallow; additional project-specific topic documents are fine. Indexes are optional navigation, never mandatory payloads.

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

## Minimal knowledge example

Illustrative template only; replace placeholders with actual evidence before saving. Omit unsupported metadata. `updated_at` is the actual write date; `verified_at` is the actual verification date, never an inferred value.

```markdown
---
id: descriptive-decision
project: example-project
type: decision
status: provisional
source: path/to/authoritative-decision.md
---
# Descriptive decision

## Summary
Concise accepted decision and relevant conditions, once confirmed.

## Evidence
Source location, acceptance evidence, rationale, and verification limits.
```

Types: `fact`, `decision`, `learning`, `reference`. States: `verified`, `provisional`, `stale`, `superseded`. Use minimal consistent YAML frontmatter when useful; tags and dates are optional. Project is omitted for global records. Important claims need traceable sources when available. For learnings include conditions, verified cause, and confirmed resolution where applicable. Avoid generic filenames such as `notes.md` or `misc.md`.

Checkpoint example (illustrative, ignored working data):

```markdown
# Resume descriptive-task
Project/repository: <resolved identity>
Goal: <requested outcome>
Status/progress: <verified work>
Files/artifacts: <paths>
Checks: <executed checks and results>
Blockers/remaining work: <current observations>
Next steps: <concise actions>
```

## Concurrent writes

All cooperating harnesses must follow this protocol for Markdown and YAML, including mapping/index edits. No database or locking service is needed.

1. Read the target and retain its exact bytes or digest (or record absence). Prepare a narrow proposed edit preserving unrelated content. Review provenance, metadata syntax, and sensitive values.
2. Acquire the store-wide lock with atomic `mkdir <memory-root>/.memory-write-lock`. Create the root first if needed. If the directory exists, stop/report contention or retry later; never remove another writer's lock. An abandoned lock requires checking ownership/process state and user approval before removal. Keep ownership information non-sensitive and release only your own lock.
3. While holding the lock, re-read the target and compare to the original snapshot, including absence. If changed, do not overwrite: release the lock, rebase the edit on the latest text, and recheck; report semantic conflicts instead of choosing a winner. Repeat for each affected file. Search for an existing canonical topic again before creating a new record to avoid duplicate creations under different names.
4. Validate the proposed YAML/frontmatter with an available parser, ensure Markdown fences/sections remain intact, and review the diff. If no parser exists, use simple metadata and inspect it explicitly; never claim parser validation. Write a unique temporary file in the target's directory, flush/close it, then use an atomic rename/replace (e.g. `os.replace` or same-filesystem `mv`). Preserve appropriate file permissions. Recheck the target immediately before replacement to detect non-cooperating edits where feasible.
5. Verify the resulting file, clean up only your own temp file, and release your lock in a finally/trap path. Update navigation under the same lock when needed. Each file replacement is atomic, not a transaction across multiple files; write knowledge before optional indexes, and report partial completion.

Lock scope covers knowledge, working state, and projects.yaml. The lock directory and its contents are ignored by Git. Atomic rename prevents partial-file reads; the lock and snapshot check prevent cooperating writers from losing updates. Uncooperative editors can still race between check and replace; report this limit. If available tools cannot support locking/atomic replacement, defer the write and report why rather than use a blind overwrite. The protocol is tool-neutral and does not scan or redact secrets automatically; the agent must review content before persistence.
