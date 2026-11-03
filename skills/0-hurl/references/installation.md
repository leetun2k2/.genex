# Install Hurl

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew install hurl
```

## Verify and configure

```sh
hurl --version
hurlfmt --version
```

The package includes `hurlfmt`. Version checks send no HTTP requests; API credentials belong to the target workflow.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://hurl.dev/docs/installation.html).
