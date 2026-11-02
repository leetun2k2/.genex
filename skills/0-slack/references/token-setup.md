# Slack API token setup

Read this when an API token is missing, the task needs a different identity, or Slack reports missing scopes. Developer CLI login and Web API tokens are separate. Reuse an appropriate existing token before proposing app installation or permission changes.

## Choose the identity

| Credential | Acts as | Use |
| --- | --- | --- |
| Bot token (`xoxb-…`) | The installed app's bot | Bot messages and supported reads within the bot's access. A bot DM is a conversation with the bot. |
| User token (`xoxp-…`) | The authorizing user | Supported actions on that user's behalf, including `search.messages`. Access still depends on scopes and conversation membership. |
| Developer CLI account (`slack auth login`) | A developer CLI identity | App and workflow management; this login alone does not supply a general-purpose message API token. |

Check the [token documentation](https://docs.slack.dev/authentication/tokens/) and the target method's documentation for supported token types. Do not infer access from a token prefix alone.

## Request only the required scopes

Choose scopes for the actual operation and conversation type; this table is a guide, not an installation checklist. Check the [method reference](https://docs.slack.dev/reference/methods/) before changing permissions.

| Operation | Relevant scopes |
| --- | --- |
| Search messages with a user token | `search:read` |
| Send messages | `chat:write` |
| Read conversation metadata | `channels:read`, `groups:read`, `im:read`, or `mpim:read`, as applicable |
| Read message history | `channels:history`, `groups:history`, `im:history`, or `mpim:history`, as applicable |
| Resolve users | `users:read`; email lookup additionally requires `users:read.email` |
| Open or resume a DM | `im:write`; multi-person DMs use `mpim:write` |

A scope does not grant access to every conversation. For thread reads, also check the token restrictions in [`conversations.replies`](https://docs.slack.dev/reference/methods/conversations.replies/).

## Create or update a token when needed

Explain the intended workspace, acting identity, app, required scopes, and installation effects. Follow the main skill's approval checkpoint before making authentication, scope, or installation changes. If workspace policy requires administrator approval, report that dependency.

1. Open [Slack app management](https://api.slack.com/apps) and select the intended app, or create one when authorized.
2. Under **OAuth & Permissions**, add only the required scopes under **User Token Scopes** or **Bot Token Scopes**, matching the chosen identity.
3. Install or reinstall the app into the intended workspace and review the consent screen. Installation may require workspace approval.
4. Obtain the corresponding **User OAuth Token** or **Bot User OAuth Token**. Have the user configure it through their local secret mechanism; never ask them to paste it into chat.

## Load credentials and verify

Provide the token to the CLI process as `SLACK_USER_TOKEN` or `SLACK_BOT_TOKEN` through the user's existing secret mechanism. Do not put literal credentials in shell commands, shell history, tracked configuration, or payload files. Do not dump environment variables to check whether a token exists.

`slack api` prefers `SLACK_BOT_TOKEN` over `SLACK_USER_TOKEN`. When using a user token, remove the bot variable for every command in that operation, including identity verification:

```sh
env -u SLACK_BOT_TOKEN slack api auth.test --method GET --skip-update --no-color
```

For a bot token:

```sh
slack api auth.test --method GET --skip-update --no-color
```

Check `ok`, `team_id`, `user_id`, and `bot_id` when present. Verify the intended workspace and identity, then perform the requested bounded read. Do not send a test message merely to validate authentication. Do not claim message access from `auth.test` alone.

If authentication fails, report the returned error without revealing the token. For missing scopes, identify the specific missing permission and its operation instead of suggesting the entire table above. If a token is exposed, recommend revocation and replacement through Slack's credential controls.
