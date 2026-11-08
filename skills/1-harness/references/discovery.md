# Canonical source and host discovery

Checked 2026-11-08 on the local macOS installation. Canonical harness sources are `skills/1-harness`, `skills/0-codex`, `skills/0-claude` and `skills/0-antigravity`. Antigravity's executable remains `agy`; the old skill directory `0-agy` is absent. Relative consumer references must point to the current names.

Resolve paths before editing; a home-directory symlink can make a native skill location the same file as canonical source. Local `~/.codex`, `~/.claude`, `~/.agents` and `~/.gemini` resolve to this Genex workspace. Consequently these user locations reach canonical `skills/`:

| Host | Relevant discovery path and local evidence |
| --- | --- |
| Codex | Documented user `~/.agents/skills`; local path is the canonical directory. `~/.codex/skills` also resolves there; don't depend on it as the only documented path. |
| Claude Code | Documented personal `~/.claude/skills`; local path is canonical. |
| Antigravity CLI | Documented shared `~/.gemini/skills`; local path is canonical. The cross-App/CLI personal `~/.gemini/config/skills` path is absent, but that does not imply CLI skills are undiscoverable. |

Repo-local `.agents/skills/` is a separate older bundle. Its obsolete `0-harness` entry previously appeared alongside canonical skills in Codex metadata. With explicit user authorization, that directory was moved outside discovery roots on 2026-11-08; do not regenerate it from stale source. Other bundled skills were left unchanged. Canonical callers continue to load the routing and execution skills directly.

The `1-code-review` discovery error was a leading blank line before its existing YAML frontmatter, not missing metadata content. Removing that blank line puts `---` at byte zero without changing the review instructions. A refreshed Codex app-server `skills/list` check confirmed zero discovery errors, enabled `1-code-review` and the four canonical harness skills, and no `0-harness` entry. A catalog already injected into a running conversation can remain stale until its host reloads it.

Use a host's supported metadata/skill listing or inspect its provided catalog before claiming effective loading. File/path checks and documentation establish discoverability, not behavior or precedence in every host/profile. For newly renamed metadata, reload using the installed host's supported mechanism or a new session. Never send an unverified slash command through model print mode merely to test discovery.

Sources: [Codex local locations](https://learn.chatgpt.com/docs/build-skills), [Claude skill locations](https://code.claude.com/docs/en/skills), [Antigravity discovery](https://antigravity.google/docs/customization-discovery?tab=cli). Actual model behavior and live Claude/Antigravity host catalogs were not tested; Codex metadata discovery was queried directly. Canonical skills are visible in this Codex session; Claude/Antigravity loading is supported by path identity and documented discovery, not a model run.
