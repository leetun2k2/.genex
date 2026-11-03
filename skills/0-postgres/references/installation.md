# Install PostgreSQL client

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew install libpq
export PATH="$(brew --prefix libpq)/bin:$PATH"
```

## Verify and configure

```sh
command -v psql
psql --version
```

`libpq` is keg-only and provides `psql` without installing a database server. Add the PATH export above to `~/.zshrc` if persistence is needed. Reuse existing connection settings and credentials; version checks need no database connection.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://formulae.brew.sh/formula/libpq).
