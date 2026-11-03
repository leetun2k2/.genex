# Install Chrome DevTools CLI

Use when `chrome-devtools` is missing. Requires a supported Node.js LTS with npm
and Google Chrome. Reuse existing installations.

On macOS, install missing prerequisites with [Homebrew](https://docs.brew.sh/Installation):

```sh
brew install node
brew install --cask google-chrome
npm install -g chrome-devtools-mcp@latest
```

Verify without launching a browser:

```sh
node --version
npm --version
command -v chrome-devtools
chrome-devtools --help
```

If the binary is missing, check that the `bin` directory under `npm prefix -g`
is on PATH. Avoid `sudo npm install`; use a writable Node installation.
Run `chrome-devtools list_pages` only when browser access is needed; it can start
the background server and browser. No MCP client registration is required for CLI use.
The CLI evolves; check leaf help against this skill before using copied flags.

Sources: [official CLI installation](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/cli.md),
[Node.js downloads](https://nodejs.org/en/download).
