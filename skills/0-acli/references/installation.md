# Install Atlassian CLI

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew tap atlassian/homebrew-acli
brew install acli
```

## Verify and configure

```sh
acli --version
acli --help
```

Reuse existing authentication; check `acli jira auth status` or `acli confluence auth status`. Installation does not authenticate an account.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://developer.atlassian.com/cloud/acli/guides/install-macos/).
