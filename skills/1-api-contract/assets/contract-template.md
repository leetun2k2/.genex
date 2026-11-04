<!-- TEMPLATE ONLY: replace bracketed labels with verified details; remove notes and unused sections before saving a contract. -->
# [Feature name] — API Contract

**Status:** Proposed  
**Client goal:** [One sentence explaining what the caller can achieve]

## Access and conventions

- Base path: `[base path]`
- Authentication: [mechanism/scope, or link to existing conventions]
- Shared error format: [link to existing error contract, if any]

<!-- Only include an index when there are multiple operations. -->
## Operations

| Endpoint | Client action |
| --- | --- |
| `[METHOD] [path]` | [One-line purpose] |

## `[METHOD] [path]` — [Action]

[One sentence explaining externally observable behavior.]

### Request

<!-- Include path/query/header fields only if relevant. -->
| Parameter | In | Type | Required | Meaning / constraints |
| --- | --- | --- | --- | --- |
| `[parameterName]` | path | string | Yes | [Client-relevant constraint] |

<!-- Include a request body only when the operation has one. -->
**Body** (`application/json`):

```json
{
  "field": "example"
}
```

| Field | Type | Required | Meaning / constraints |
| --- | --- | --- | --- |
| `field` | string | Yes | [Meaning and confirmed constraints] |

### Success — `[HTTP status]`

```json
{
  "id": "example-id"
}
```

<!-- Keep response fields only when their meaning or constraints are not self-evident from a simple example. -->
| Field | Type | Meaning / constraints |
| --- | --- | --- |
| `id` | string | [Identifier meaning] |

### Errors

| HTTP status | Error code (if supported) | Meaning / client action |
| --- | --- | --- |
| `[status]` | `[stable-code]` | [When it occurs and what the client should do] |

<!-- Keep only real behavior affecting integration: null/omission, paging, state changes, async completion, retries, idempotency, etc. -->
### Behavior client must know

- [Material externally observable rule]

<!-- Only include this section if an unresolved decision blocks correct client integration. -->
## Open decisions

| Decision | Client impact | Question |
| --- | --- | --- |
| TBD | [What the client cannot safely assume] | [Precise question to decide] |
