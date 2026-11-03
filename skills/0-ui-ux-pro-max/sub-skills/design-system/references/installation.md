# Install Node.js and Python 3

Read when the runtime required by the selected workflow is missing. Reuse existing
installations. On macOS, with [Homebrew](https://docs.brew.sh/Installation):

```sh
brew install node python
```

Verify:

```sh
node --version
python3 --version
```

The bundled token and slide helpers use standard runtime libraries. Use `python3` for examples written with `python` when that alias is unavailable.

Run bundled scripts from the actual installed skill directory, not an assumed
`.claude/skills` example path. If a command is missing after installation, reopen
the shell and check PATH. Runtime installation does not install companion skills.

Sources: [Node.js downloads](https://nodejs.org/en/download),
[Python downloads](https://www.python.org/downloads/).
