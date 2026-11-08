# Genex

Shared rules and skills for Claude Code, Codex, and Agy. `CLAUDE.md`,
`AGENTS.md`, `CODEX.md`, and `GEMINI.md` link to `GENEX.md`, which contains
the canonical [memory policy](GENEX.md). Memory is a knowledge cache;
rules, skills, and task workflows remain separate.

The existing local installation links `~/.genex`, `~/.claude`, `~/.codex`,
and `~/.gemini` to this directory. Preserve that setup when installing
elsewhere, or configure native instruction/skill discovery to point here.
Claude/Codex share `skills/`; Antigravity CLI also reaches it through
`~/.gemini/skills` (its shared CLI discovery path). These user paths resolve
to canonical source. The obsolete repo-local `.agents/skills/0-harness` bundle has been removed.
Use canonical `skills/1-harness/SKILL.md` and the selected execution skill;
do not recreate the old bundle. See [harness discovery evidence](skills/1-harness/references/discovery.md).
If native discovery is unavailable, explicitly ask the harness to read
`GENEX.md` and `skills/0-memory/SKILL.md` using absolute paths.

The canonical store is `memories/`. Git tracks only the explicitly allowlisted
`*.example.md` and `projects.example.yaml` templates showing its structure.
Actual indexes, repository mappings, knowledge, checkpoints, locks, and temporary
files stay local and are ignored. Existing local data is preserved.
For a new installation, copy `memories/projects.example.yaml` to
`memories/projects.yaml` and replace illustrative values with confirmed mappings.
Create real knowledge files only when useful; examples are never evidence.
See [storage conventions](skills/0-memory/references/storage.md) for the full
logical tree and write procedure. Harness-native memory remains independent.

The shared policy requires producers to save canonical deliverables and valuable
reviews, while allowing ephemeral outputs to remain in chat. Producers save and
verify files independently of orchestrators. At meaningful completion, use
[0-memory](skills/0-memory/SKILL.md) when useful for durable knowledge and selective
artifact references; full documents stay in their canonical location. Saving,
approval, Git commits, and memory indexing are separate actions. Real memory is
local-only and has no inherent backup or cross-machine synchronization.

This is instruction-driven automation: native skill selection varies,
permissions may prevent writes, and abrupt termination
cannot guarantee a checkpoint. No mandatory hook or background process is
installed. Confirm rule loading and skill discovery in each harness/version.

Concurrent writers use the documented directory lock, snapshot comparison,
and same-directory atomic replacement. All writers must cooperate; external
editors are not forced to take the lock. Review content for secrets before
persistence. Markdown/YAML remain canonical; future search indexes can be
rebuilt without changing storage.

## Passive English Coach (Phase 1)

The focused `Passive English Coach — Balanced` section in `GENEX.md` provides
occasional corrections after the main answer, with one corrected sentence and
a short Vietnamese explanation. It preserves engineering shorthand and skips
strict output formats and generally urgent work. There is no skill, hook,
extra model call, dependency, or learning history. Keeping the rule inline
reuses the shared loader without cross-harness imports or extra file reads.

Local loading paths checked on 2026-11-05:

| Harness | Global entry point resolving to `GENEX.md` | Loading mechanism checked |
| --- | --- | --- |
| Codex | `~/.codex/AGENTS.md` | [Global instruction discovery](https://learn.chatgpt.com/docs/agent-configuration/agents-md); `AGENTS.override.md` takes precedence; a custom `CODEX_HOME` changes the location. |
| Claude Code | `~/.claude/CLAUDE.md` | [User instructions](https://code.claude.com/docs/en/memory); loaded at session start. |
| Agy (Antigravity CLI) | `~/.gemini/AGENTS.md` and `~/.gemini/GEMINI.md` | [Global rule discovery](https://antigravity.google/docs/rules); installed binary's embedded rule guide also recognizes these filenames and deduplication. |

All these entry points resolve through the existing home-directory and
instruction-file symlinks. `CODEX.md` remains a compatibility alias; it is not
the default Codex global entry point. Gemini CLI is not installed here and its
loading was not verified; no support is claimed for arbitrary other harnesses.
Alternate profiles, instruction exclusions, higher-priority language rules,
and context limits can prevent the coach from applying. Claude Cowork has
additional symlink restrictions; it is not covered by the Claude Code check.
Start a new session to load changed instructions. This is best-effort prompt
guidance, not deterministic enforcement; no live model behavior test was run.

Manual review of the rule against the requested scenarios (not model executions):

| Input or context | Rule review result |
| --- | --- |
| `Review this PR` | Explicitly accepted; no tip. |
| `Fix bug and run tests` | Explicitly accepted shorthand; no tip. |
| `I want agent review this PR before merge` | Eligible: “I want the agent to review this PR before merging.” Why: “Cần mạo từ trước ‘agent’ và ‘to’ sau ‘want’.” |
| `Please check why this query running slow` | Eligible: “Please check why this query is running slowly.” Why: “Dùng ‘is running’ cho hành động đang diễn ra và ‘slowly’ để bổ nghĩa cho động từ.” |
| `Investigate the race condition` | Explicitly accepted; no tip. |
| Vietnamese containing `race condition` | Isolated technical terms excluded; no tip. |
| Code blocks or logs | Excluded; only independently authored English prose remains eligible. |
| JSON-only request | Strict format excludes the tip entirely. |
| Urgent production incident | Task takes priority; generally omit tips. |
| Correct, natural English | Explicitly excluded; no tip. |
