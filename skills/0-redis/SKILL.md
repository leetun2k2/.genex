---
name: 0-redis
description: Connect to Redis with redis-cli and query keys, TTLs, and server stats to test or debug connection and data issues.
---

# Redis

## Installation

If a required CLI or runtime is missing, read [installation and verification](references/installation.md).


Use the installed `redis-cli`. Choose commands based on the user's question; use `redis-cli --help` when needed.

## Read and write policy

- Run in-scope read-only operations without user confirmation. Keep filters, limits, and timeouts as operational safeguards.
- Before any write, mutation, or configuration change, including local edits, prepare a concrete change set. Immediately before execution, show the target, exact commands with secrets redacted, and a diff or effects summary when no text diff is available. Ask for explicit user confirmation and wait for it. The initial task request alone does not satisfy this checkpoint.
- Confirmation covers the displayed change set, including repetitive sub-steps within it. Ask again only if the target, proposed changes, or side effects change.

## Inputs

Resolve these from the request, environment, or project config. Ask only when a missing fact prevents useful work.

| Input | Details |
| --- | --- |
| Target | Environment and host/port, socket, or URI; DB index; authentication and TLS if needed. Use local defaults only when local Redis is intended. |
| Goal | What to test/debug, with an exact key, pattern, or command when relevant. |
| Write scope | For writes: allowed target, operation, and keys. Inspection is read-only by default. |

## Usage

- Reuse existing connection config. Prefer `REDISCLI_AUTH` for passwords; keep secrets out of command arguments, logs, and files. Add TLS flags as needed; keep certificate verification enabled.
- Use one-shot commands with `-e` for command error exit codes. Set a total runtime limit through the execution runner, or `timeout`/`gtimeout` if installed. `-t` is the connection timeout in the installed CLI; repeat count is `-r`, interval is `-i`.
- Choose reads based on key type and size. Prefer fields, ranges, and cursor scans over full payloads. Use `SCAN` instead of `KEYS` on shared targets; `COUNT` is a hint, and iteration finishes at cursor `0`.
- For Redis Cluster, use `-c` and DB `0`. Scans and server stats apply to the connected node, not the whole cluster.
- Include all mutating commands, scripts, configuration changes, `FLUSHALL`, `FLUSHDB`, and disruptive actions such as `DEBUG SEGFAULT` in the write confirmation checkpoint. For a write/read probe, preview the unique debug key, `SET ... NX EX 60` command, and expiry effect and obtain confirmation; verify only after `OK` and let it expire.
- `KEYS *` and `MONITOR` are reads and need no user confirmation. Prefer `SCAN` on shared targets because `KEYS *` can block the server; a client timeout does not stop that server work. Use `MONITOR` only for a bounded interval when needed, stop it promptly, and redact sensitive command payloads. Both can add significant load.
- Report the target without secrets, commands used, key results, and any limits. Separate findings from possible causes; stop when the question is answered.

## Examples

Adapt connection flags and queries to the actual target. These examples use macOS zsh/bash.

```sh
# Local Redis; replace flags for the resolved target.
redis_cmd=(redis-cli -h 127.0.0.1 -p 6379 -n 0 -t 5 -e --no-raw)
"${redis_cmd[@]}" PING

# Inspect a known key; read the range only if TYPE is string.
"${redis_cmd[@]}" TYPE 'cache:order:123'
"${redis_cmd[@]}" TTL 'cache:order:123'
"${redis_cmd[@]}" MEMORY USAGE 'cache:order:123'
"${redis_cmd[@]}" GETRANGE 'cache:order:123' 0 1023

# One scan page; continue with the returned cursor only if needed.
"${redis_cmd[@]}" SCAN 0 MATCH 'cache:order:*' COUNT 100

# Choose examples that match the key type: hash, list, or sorted set.
"${redis_cmd[@]}" HGET 'order:123' status
"${redis_cmd[@]}" HMGET 'order:123' status updated_at
"${redis_cmd[@]}" LLEN 'queue:orders'
"${redis_cmd[@]}" LRANGE 'queue:orders' 0 19
"${redis_cmd[@]}" ZRANGE 'leaderboard' 0 19 WITHSCORES

# Inspect server memory.
"${redis_cmd[@]}" INFO memory
```

For authenticated TLS, assuming `REDIS_HOST` and `REDISCLI_AUTH` are already set:

```sh
redis-cli -h "$REDIS_HOST" -p 6379 --user app-debug --tls -t 5 -e PING
```

Example request: “Use staging config to inspect the TTL and value of `cache:order:123` and explain whether it matches the expected cache state.”
