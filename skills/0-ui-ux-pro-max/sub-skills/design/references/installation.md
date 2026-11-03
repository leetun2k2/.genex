# Install Python 3

Read when the runtime required by the selected workflow is missing. Reuse existing
installations. On macOS, with [Homebrew](https://docs.brew.sh/Installation):

```sh
brew install python
```

Verify:

```sh
python3 --version
```

Search helpers use bundled data. Image generation additionally requires `google-genai` and Pillow in a virtual environment:

```sh
python3 -m venv .venv
.venv/bin/python -m pip install google-genai pillow
.venv/bin/python -c "from google import genai; from PIL import Image"
```

Create the environment in the chosen working directory and use its Python for generation scripts. Reuse an existing environment when available. Reuse `GEMINI_API_KEY` without printing it; dependency checks do not call the API or verify credentials. See [Google Gen AI SDK](https://googleapis.github.io/python-genai/).

Run bundled scripts from the actual installed skill directory, not an assumed
`.claude/skills` example path. If a command is missing after installation, reopen
the shell and check PATH. Runtime installation does not install companion skills.

Sources: [Node.js downloads](https://nodejs.org/en/download),
[Python downloads](https://www.python.org/downloads/).
