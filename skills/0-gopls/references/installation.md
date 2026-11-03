# Install gopls

Use when `gopls` is missing. Requires a supported Go toolchain; reuse the project's
configured Go version. On macOS, install Go if needed using
[Homebrew](https://docs.brew.sh/Installation):

```sh
brew install go
go install golang.org/x/tools/gopls@latest
```

`go install` puts the binary in `GOBIN`, or the first GOPATH entry's `bin` directory
when `GOBIN` is unset. Inspect those settings:

```sh
go version
go env GOBIN GOPATH
```

Add the resolved binary directory to PATH (and `~/.zshrc` for persistence).
For the default GOPATH this is `$HOME/go/bin`. Then verify:

```sh
command -v gopls
gopls version
gopls help
```

Bare `gopls` starts a server; use explicit subcommands for verification.
Use a project-required gopls version instead of `@latest` when one is pinned.

Source: [official gopls installation](https://go.dev/gopls/#installation).
