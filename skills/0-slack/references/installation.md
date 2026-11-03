# Install Slack CLI

Use when the public Slack CLI (`slack`) is missing. This skill expects Slack's
official developer CLI, including `slack api`, rather than a third-party namesake.
Requires Git (`git --version`); on macOS install it with `brew install git` if missing.

Install using Slack's macOS/Linux installer:

```sh
curl -fsSL https://downloads.slack-edge.com/slack-cli/install.sh | bash
```

Alternatively, download the archive matching Apple Silicon or Intel from the
[official installation guide](https://docs.slack.dev/tools/slack-cli/guides/installing-the-slack-cli-for-mac-and-linux/)
and place the extracted binary on PATH.

Verify:

```sh
command -v slack
slack version --skip-update
slack api --help
```

If not found, add `$HOME/.local/bin` to PATH in `~/.zshrc` and open a new shell.
If another `slack` binary exists, resolve that collision before installing.
Installation does not authenticate developer accounts or provision Web API tokens.
For missing API credentials, read [token setup](token-setup.md); reuse existing
credentials and follow the skill's authentication policy.
