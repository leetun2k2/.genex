# Install GitHub CLI

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew install gh
```

## Verify and configure

```sh
gh --version
git --version
```

Reuse existing authentication and inspect it with `gh auth status`. Install Git separately if missing (`brew install git`). Do not start login just to verify installation.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://cli.github.com/).
