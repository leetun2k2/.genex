# Install CLI harnesses

Install only the harness selected by the calling workflow. Reuse an existing
working installation. These commands target macOS; Homebrew commands require
[Homebrew and its shell setup](https://docs.brew.sh/Installation).

## Codex

```sh
brew install --cask codex
codex --version
codex exec --help
```

Use the [official Codex CLI guide](https://developers.openai.com/codex/cli/)
for authentication and alternative installation. The [Homebrew cask](https://formulae.brew.sh/cask/codex) documents the command above. Existing account setup should be reused.

## Claude Code

```sh
brew install --cask claude-code
claude --version
claude --help
```

See [official setup](https://code.claude.com/docs/en/setup).
Homebrew installations are updated with `brew upgrade --cask claude-code`.
Complete initial account setup interactively only when needed for the requested task.

## Antigravity (`agy`)

```sh
curl -fsSL https://antigravity.google/cli/install.sh | bash
export PATH="$HOME/.local/bin:$PATH"
agy --version
agy --help
```

The default binary is `~/.local/bin/agy`. Persist that PATH entry in `~/.zshrc`
if needed. Follow the [official CLI getting started guide](https://antigravity.google/docs/getting-started?tab=cli)
for initial authentication and configuration; this is the CLI, not an editor shell launcher.

## Verify readiness

Use `command -v` for the selected binary. Reopen the shell after PATH changes.
Version/help checks verify installation only; they do not prove account access
or that a model invocation works. Return to `SKILL.md` for unattended execution,
permissions, and output capture. Do not invoke an agent merely to check installation.
