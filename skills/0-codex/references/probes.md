# Codex metadata probes

Checked against official app-server documentation on 2026-11-08. No model turn is needed. Use an existing supported client when available; otherwise start `codex app-server` with an outer timeout, send newline-delimited JSON on stdin and read JSON replies from stdout. Keep stderr separate.

1. Send `{"id":1,"method":"initialize","params":{"clientInfo":{"name":"harness_probe","version":"1.0"}}}` and wait for response id 1.
2. Send notification `{"method":"initialized"}`.
3. Send `{"id":2,"method":"account/rateLimits/read"}` and wait for response id 2. Handle API/provider/auth errors as unknown/unavailable; do not initiate login.
4. Optionally send `{"id":3,"method":"model/list","params":{}}` for model/effort discovery; respect pagination if returned.
5. Close stdin, wait briefly and terminate only the probe child process if it does not exit. Never start a thread/turn, redeem reset credits or invoke account mutations.

Ignore unrelated notifications while matching response IDs. Do not forward account identity or full raw responses into shared reports. Normalize quota windows from `rateLimitsByLimitId`, or the legacy `rateLimits` fallback, preserving pool IDs, percentages, reset times and any additional applicable limits. `usedPercent` is consumed, not remaining. Missing or changed fields stay unknown; do not infer infinity.

Reference: [official app-server protocol](https://learn.chatgpt.com/docs/app-server). This handshake, quota read and model list sequence also succeeded against the local CLI on 2026-11-08. This proves metadata access at that time, not future execution or quota availability.
