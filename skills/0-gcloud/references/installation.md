# Install Google Cloud CLI

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew install --cask gcloud-cli
```

## Verify and configure

```sh
gcloud --version
```

Reuse existing configuration. `gcloud auth list --filter=status:ACTIVE --format="value(account)"` checks the active CLI identity. Application Default Credentials are separate. Follow the cask caveats if SDK companion tools need additional PATH entries.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://formulae.brew.sh/cask/gcloud-cli).
