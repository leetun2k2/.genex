# Install Node.js/npm and optional Python 3

Read when the runtime required by the selected workflow is missing. Reuse existing
installations. On macOS, with [Homebrew](https://docs.brew.sh/Installation):

```sh
brew install node
```

Verify:

```sh
node --version
npm --version
npx shadcn@latest --help
```

Use the project's existing package manager and lockfile. `npx` downloads the shadcn CLI on demand; no global shadcn installation is needed. Run `init` or `add` only for the requested project changes. Optional Python helpers require `brew install python`; verify with `python3 --version`. See the [official shadcn CLI guide](https://ui.shadcn.com/docs/cli).

Run bundled scripts from the actual installed skill directory, not an assumed
`.claude/skills` example path. If a command is missing after installation, reopen
the shell and check PATH. Runtime installation does not install companion skills.

Sources: [Node.js downloads](https://nodejs.org/en/download),
[Python downloads](https://www.python.org/downloads/).
