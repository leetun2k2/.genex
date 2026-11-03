# Install Python 3 and optional Node.js

Read when the runtime required by the selected workflow is missing. Reuse existing
installations. On macOS, with [Homebrew](https://docs.brew.sh/Installation):

```sh
brew install python
```

Verify:

```sh
python3 --version
```

The screenshot workflow also uses Node.js; install it if needed with `brew install node` and verify `node --version`. The referenced `ai-artist`, `ai-multimodal`, and `chrome-devtools/scripts/screenshot.js` are companion skill resources, not packages installed by Python or npm. Verify those resources exist before choosing that workflow. They are not bundled in this repository. Use an available image/browser tool when suitable, or report the missing companion dependency; do not invent a package install command. For the available browser CLI, see [Chrome installation](../../../../0-chrome/references/installation.md).

Run bundled scripts from the actual installed skill directory, not an assumed
`.claude/skills` example path. If a command is missing after installation, reopen
the shell and check PATH. Runtime installation does not install companion skills.

Sources: [Node.js downloads](https://nodejs.org/en/download),
[Python downloads](https://www.python.org/downloads/).
