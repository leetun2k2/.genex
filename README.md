# Genex

Shared rules and skills for Claude Code, Codex, and Agy. `CLAUDE.md`,
`AGENTS.md`, `CODEX.md`, and `GEMINI.md` link to `GENEX.md`, which contains
the canonical [memory policy](GENEX.md). Memory is a knowledge cache;
rules, skills, and task workflows remain separate.

The existing local installation links `~/.genex`, `~/.claude`, `~/.codex`,
and `~/.gemini` to this directory. Preserve that setup when installing
elsewhere, or configure native instruction/skill discovery to point here.
Claude/Codex share `skills/`; Agy reaches the same skill through the
canonical instruction reference. No skill implementation is copied.
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
