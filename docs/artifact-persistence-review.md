# Artifact persistence implementation review

Date: 2026-11-04

Implementation scope authorized by the user's “Improve Artifact Persistence Across Genex Skills” request, following the preceding review proposal. Changes are applied locally; this report is not a separate content approval, Git commit, or deployment authorization.

## Verified findings

- `GENEX.md` is the canonical instruction source. `AGENTS.md`, `CLAUDE.md`, `CODEX.md`, and `GEMINI.md` are symlinks to it. The local `~/.genex`, `~/.codex`, `~/.claude`, and `~/.gemini` routes resolve to this workspace. This conversation supplied AGENTS instructions and the skill catalog; the subsequent instruction refresh included the new persistence policy. Alternate harness runtime loading was not independently tested.
- Before this change, effective global rules required continuity before handoff/compaction but did not contain the completion-time durable-memory evaluation described by README. README alone was insufficient evidence of effective behavior.
- API contracts, release handoffs, and document briefs already explicitly saved files. Test execution already saved a separate report, but lacked a complete saved-path/update/failure contract. Test design specified Markdown and placement without explicitly making saving a completion requirement.
- Technical/task designs delegated context persistence to callers without clearly distinguishing saving their own deliverables. Requirement findings, acceptance/code reviews, and final debate conclusions lacked explicit persistence contracts. Debate round files did not ensure a final review brief was saved.
- Canonical UI design-system persistence was optional. Component specifications/token designs and HTML slide deliverables lacked explicit save-and-verify completion contracts. Brand updates and generated visual assets already targeted project files.
- `0-memory` already supported project identity, references, complete technical artifacts, provenance, checkpoints, and safe concurrent writes. Real memory is Git-ignored local data; it has no inherent backup, version history, or cross-machine synchronization.
- `0-skill-improvement` prohibited all persistent review writes, preventing a proposal from surviving the conversation without approval.
- No established general documentation directory was present at the workspace root. The repository used an allowlist-style `.gitignore`, so merely creating `docs/` would have left this report untracked by default.

## Implemented policy and ownership

Canonical specifications, technical/task/test designs, contracts, and implementation plans must be saved before their deliverable is complete. Reviews and reports are saved when useful for approval, audit, handoff, follow-up, or long-term reference. Quick explanations and trivial observations can remain in chat. Classify by the purpose of the actual output, rather than skill name: requirement evaluation remains findings, not an automatically authored requirements specification.

Resolve locations in this order: explicit user path; intended existing artifact when revising; repository conventions; appropriate workspace documentation directory. Update canonical documents rather than making redundant versions. Existing version control can preserve history; no automatic commit is added.

The producer saves and verifies its own deliverable and returns the actual path, independently of an orchestrator. Failed persistence must be reported accurately. An orchestrator may coordinate workflow, but is not required for saving. Saving, approving content, committing, and memory indexing remain separate operations. Drafts retain their actual review status.

## Files changed

| Files | Purpose |
|---|---|
| `GENEX.md` | Concise common classification, location, ownership, verification, approval, and memory principles |
| `README.md` | Align documented behavior with the effective policy and local-memory limitations |
| `.gitignore` | Allowlist only this durable report under `docs/`; private memory remains ignored |
| `skills/0-memory/SKILL.md`, `skills/0-memory/references/storage.md` | Selective pointers, non-duplicating storage, existing project-scoped fallback, and durability limitations |
| `skills/0-skill-improvement/SKILL.md` | Allow saving proposals/references before approval; retain approval for changing effective skills/rules/configuration |
| `skills/1-technical-design/SKILL.md`, `skills/1-task-design/SKILL.md`, `skills/1-test-design/SKILL.md` | Save canonical Markdown deliverables, revise existing paths, verify persistence, report failures |
| `skills/1-requirement-analysis/SKILL.md` | Save valuable findings independently of workflow state |
| `skills/1-code-review/SKILL.md`, `skills/1-acceptance-review/SKILL.md`, `skills/1-debate/SKILL.md` | Conditional persistence for valuable reviews/final conclusions |
| `skills/1-test-execution/SKILL.md` | Complete the existing saved report contract with revision, path, and failure reporting |
| `skills/0-ui-ux-pro-max/SKILL.md` | Require persistence when the requested output is a canonical design system |
| `skills/0-ui-ux-pro-max/sub-skills/design-system/SKILL.md`, `skills/0-ui-ux-pro-max/sub-skills/slides/SKILL.md` | Save requested component/token specifications and HTML presentations |
| `docs/artifact-persistence-review.md` | Durable findings, implementation scope, verification, and limitations |
| Local `memories/knowledge/projects/genex/references.md`, `memories/INDEX.md` | Discoverable project reference to this report; not a copy of it |

## Skills intentionally unchanged

- `1-api-contract`, `1-document-release`, and `0-document-brief`: existing save/location/update mechanisms are sufficient alongside the shared policy. Their specialized file conventions remain intact.
- `1-query-log`: a simple query or short investigation can remain ephemeral; significant incident findings are covered by the shared policy without requiring a file for every query.
- `brand`, `design`, `banner-design`, and `ui-styling`: established source files, generated assets, or repository-native code remain the deliverables. Shared location, failure, and approval principles apply without introducing companion reports.
- Tool capabilities (`0-acli`, `0-chrome`, `0-gcloud`, `0-github`, `0-gopls`, `0-harness`, `0-hurl`, `0-kubectl`, `0-postgres`, `0-redis`, `0-slack`) retain existing interfaces and authorization requirements. Saving every read/tool interaction would create unnecessary documents.
- Cached plugins, synced external packages, system skills, generated copies, and harness symlinks were not edited. Reusable Genex sources own these changes.

## Memory discovery and lifecycle

The existing repository mapping resolves the normalized origin `github.com/leetun2k2/.genex` to project `genex`, including the configured SSH alias. The project reference points to this report with its purpose, verification provenance, and relevant review status. The memory index provides a navigation link; project-scoped search remains sufficient without mandatory indexes.

Future sessions start at `~/.genex/memories/INDEX.md`, resolve the project using `projects.yaml`, and retrieve the relevant reference. They must inspect the target/current source before relying on cached claims. A verified reference confirms location/provenance, not approval of the document.

When repository/workspace storage is unavailable, existing `knowledge/projects/<project-id>/` storage may hold the complete artifact after identity resolution. The producer must disclose the fallback and any unmet explicit destination. Checkpoints preserve unfinished work and link complete artifacts; they do not replace them.

## Verification and outcomes

Executed checks:

- Reviewed the actual diff and ran `git diff --check` successfully.
- Ran the existing `skills/.system/skill-creator/scripts/quick_validate.py` validation function against all 13 changed skills. Ten passed. Three had unchanged baseline incompatibilities, confirmed by running the same validator against their HEAD content: `design-system` and `slides` use `argument-hint`, which this validator rejects; `1-code-review` starts with a blank line, which it rejects as missing frontmatter. No new validator failure was introduced. PyYAML parsed every changed skill's frontmatter after allowing the existing leading whitespace for that separate syntax check.
- Verified all four instruction symlinks resolve to `GENEX.md`, and tracked memory templates remain unchanged.
- Ran a temporary workspace filesystem smoke check: saved Markdown, revised the same path, confirmed one document remained, and observed `IsADirectoryError` for an invalid write target. Temporary files were removed. This checks filesystem behavior, not autonomous skill compliance.

| Required scenario | Evidence and validation boundary |
|---|---|
| Technical design produces durable Markdown | Explicit save-before-completion contract inspected; no independent skill run performed |
| Re-running updates the intended artifact | Revision contract inspected; same-path filesystem smoke check passed; no autonomous rerun performed |
| Trivial code review creates no unnecessary file | Conditional contract explicitly permits chat; static review only |
| Improvement proposal can be saved without applying changes | Proposal exception and retained mutation gate inspected; no fresh unapproved review invocation performed |
| Future session can discover an artifact | Actual memory index/reference/target chain created and read back; project search verified; future session not launched |
| Failed write cannot be reported as successful | Failure contracts inspected and invalid filesystem write observed; future agent reporting is instruction-driven |
| Saved draft is not approved | Global and producer boundaries inspected; no approval inferred from this saved report |
| Standalone producer can save output | Producer ownership explicit and independent of orchestration; static review only |

No new unit tests, dependencies, database, registry, generic artifact skill, hook, service, or orchestration layer was introduced. No commit was made.

## Remaining limitations

These are instruction changes, not an enforced persistence runtime. Abrupt interruption can still precede a save. Alternate harness behavior and autonomous scenario execution remain unverified. The three pre-existing validator incompatibilities were left unchanged to avoid unrelated modifications. Local memory still needs separately authorized backup/synchronization if protection against machine loss is required. The report is trackable but uncommitted; being saved or trackable does not itself provide remote backup.
