# Install Antigravity CLI

Reuse an existing working installation. Detect the platform first; the commands below target macOS. Homebrew commands require [Homebrew](https://docs.brew.sh/Installation). Install only when required for the authorized task.

```sh
curl -fsSL https://antigravity.google/cli/install.sh | bash
export PATH="$HOME/.local/bin:$PATH"
agy --version
agy --help
```

The default binary is `~/.local/bin/agy`. Persist that PATH entry in `~/.zshrc`
if needed. Follow the [official CLI getting started guide](https://antigravity.google/docs/getting-started?tab=cli)
for initial authentication and configuration; this is the CLI, not an editor shell launcher.

Verify `command -v agy` and `agy --version`. Installation/help checks do not prove account access or a successful model invocation. Reuse configured authentication; do not invoke a model merely to check installation. Return to [execution instructions](../SKILL.md).
