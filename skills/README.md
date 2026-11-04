# Genex — `1-api-contract`

A stateless, client-first skill for writing platform-independent API contracts in concise Markdown.

## Install in the current repository

Copy `skills/1-api-contract/` to the equivalent skill folder in your Genex workspace, or to the shared Genex skill root used by your harness. The skill writes **contract outputs** inside the currently active project, under `docs/api-contracts/<feature-slug>.md` unless the repository has an existing API documentation convention.

This bundle does not publish documents, generate OpenAPI, or mutate API implementation code.

## Example invocation

> Use `1-api-contract` to write the client-facing contract for the order-creation feature from the approved technical design and current routes. Save it in this workspace as Markdown, without generating Swagger/OpenAPI or publishing anything.
