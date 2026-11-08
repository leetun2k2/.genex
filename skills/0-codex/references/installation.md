# Install Codex

Reuse an existing working installation. Detect the platform first; the commands below target macOS. Homebrew commands require [Homebrew](https://docs.brew.sh/Installation). Install only when required for the authorized task.

```sh
brew install --cask codex
codex --version
codex exec --help
```

Use the [official Codex CLI guide](https://developers.openai.com/codex/cli/)
for authentication and alternative installation. The [Homebrew cask](https://formulae.brew.sh/cask/codex) documents the command above. Existing account setup should be reused.

Verify `command -v codex` and `codex --version`. Installation/help checks do not prove account access or a successful model invocation. Reuse configured authentication; do not invoke a model merely to check installation. Return to [execution instructions](../SKILL.md).
