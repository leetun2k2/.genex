# Install Redis CLI

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew install redis
```

## Verify and configure

```sh
command -v redis-cli
redis-cli --version
```

The package includes the server and client. A remote connection only needs `redis-cli`; do not start a local Redis service for installation verification. Reuse existing connection settings.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://formulae.brew.sh/formula/redis).
