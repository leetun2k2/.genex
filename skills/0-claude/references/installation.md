# Install Claude

Reuse an existing working installation. Detect the platform first; the commands below target macOS. Homebrew commands require [Homebrew](https://docs.brew.sh/Installation). Install only when required for the authorized task.

```sh
brew install --cask claude-code
claude --version
claude --help
```

See [official setup](https://code.claude.com/docs/en/setup).
Homebrew installations are updated with `brew upgrade --cask claude-code`.
Complete initial account setup interactively only when needed for the requested task.

Verify `command -v claude` and `claude --version`. Installation/help checks do not prove account access or a successful model invocation. Reuse configured authentication; do not invoke a model merely to check installation. Return to [execution instructions](../SKILL.md).
