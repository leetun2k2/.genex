# Install search runtime

The bundled search CLI requires Python 3; it is not a separate published CLI package.
Use when `python3` is missing. On macOS, with
[Homebrew](https://docs.brew.sh/Installation):

```sh
brew install python
python3 --version
```

On Ubuntu/Debian use `sudo apt install python3`; on Windows install a supported
Python 3 release from [Python downloads](https://www.python.org/downloads/) and
use `python` where the skill examples use `python3`.

Run from this skill's directory to verify the bundled entrypoint:

```sh
python3 scripts/search.py --help
```

Search uses the standard library and bundled CSV data; no pip installation is
needed. Resolve the actual installed skill directory instead of assuming the
example `~/.claude/skills/ui-ux-pro-max` path exists.
