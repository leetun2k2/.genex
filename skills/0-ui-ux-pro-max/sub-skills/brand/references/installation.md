# Install Node.js

Read when the runtime required by the selected workflow is missing. Reuse existing
installations. On macOS, with [Homebrew](https://docs.brew.sh/Installation):

```sh
brew install node
```

Verify:

```sh
node --version
npm --version
```

The bundled `.cjs` helpers use Node built-ins and need no global npm packages.

Run bundled scripts from the actual installed skill directory, not an assumed
`.claude/skills` example path. If a command is missing after installation, reopen
the shell and check PATH. Runtime installation does not install companion skills.

Sources: [Node.js downloads](https://nodejs.org/en/download),
[Python downloads](https://www.python.org/downloads/).
