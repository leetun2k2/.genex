# Confluence commands

Checked with local `acli 1.3.39-stable --help`. Replace sample IDs and keys. Run only commands needed for the request.

## Read pages and spaces

```sh
acli confluence space list --keys TEAM --limit 10 --json
acli confluence space view --id 12345 --json
acli confluence page view --id 123456789 --body-format storage \
  --include-version --json
```

- `space list --keys` filters by space keys; `space view --id` needs a numeric space ID. Space mutations such as archive use keys according to their help. Check the chosen leaf rather than interchanging keys and IDs.
- Page `--body-format` can request `storage`, `atlas_doc_format`, or `view`. Select the format needed to inspect the content; do not treat storage XHTML or ADF JSON as Markdown.
- Page reads can include version, labels, direct children, operations, or properties through specific `--include-*` flags. Request only relevant details. `--version` reads a specific version; `--get-draft` requests an accessible draft.
- `space list` defaults to current spaces and a limit of 50. It supports `--status`, `--type`, `--keys`, and `--expand`; no `--paginate` flag was shown. Do not claim a bounded result contains every space.

## Capability boundary

In this installed version, `acli confluence page --help` exposes only `view`: no page search/list, create, or update command is advertised. Do not invent those commands or publish a blog post as a substitute for a requested page. Recheck installed help; if still unsupported, report the limit and use an available Confluence integration capable of the requested action within the user's scope.

`space` supports `list`, `view`, `create`, `update`, `archive`, and `restore`. `blog` supports `list`, `view`, and `create`. Check each leaf's help before use; do not copy page flags to blog commands.

## Create a blog post

For a requested blog post, prepare XHTML content and resolve the space ID. Show the target, command, content, and draft/publication effects and receive explicit user confirmation before executing:

```sh
acli confluence blog create --space-id 12345 --title 'Release notes' \
  --status draft --from-file /path/to/blog-content.html --json
```

Choose status from the user's intent: `draft` saves a draft; `current` publishes and is the CLI default. Use `--private` only when requested. `--body` expects Confluence storage XHTML, while `--from-file` accepts plain text or HTML. `--from-json` and `--generate-json` are available for structured input. Check `blog view --help` and read the resulting post to verify it.
