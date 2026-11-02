---
name: 0-gcloud
description: Use gcloud to inspect Google Cloud resources, read logs, debug service and access issues, and carry out requested resource changes.
---

# Google Cloud

Use the installed `gcloud`. Choose commands based on the user's goal; use `gcloud <group> <command> --help` when needed. Prefer stable commands unless the task needs a beta or alpha feature.

## Read and write policy

- Run in-scope read-only operations without user confirmation. Keep filters, limits, and timeouts as operational safeguards.
- Before any write, mutation, or configuration change, including local edits, prepare a concrete change set. Immediately before execution, show the target, exact commands with secrets redacted, and a diff or effects summary when no text diff is available. Ask for explicit user confirmation and wait for it. The initial task request alone does not satisfy this checkpoint.
- Confirmation covers the displayed change set, including repetitive sub-steps within it. Ask again only if the target, proposed changes, or side effects change.

## Inputs

Resolve these from the request, project config, or existing CLI configuration. Ask only when a missing fact prevents useful work.

| Input | Details |
| --- | --- |
| Target | Environment, project ID, account/configuration, and region/zone/location where required. Verify the intended target rather than relying on active defaults. |
| Goal | What to inspect, debug, or change; service/resource name, expected behavior, and log time window when relevant. |
| Change scope | For changes: allowed resources, operation, desired settings, and recovery plan where relevant. Investigation is read-only by default. |

## Usage

- Reuse existing authentication. CLI credentials and Application Default Credentials are separate; check the identity used by the failing tool. Keep tokens, service account keys, secret payloads, and sensitive logs out of output and files. Avoid `--log-http` for routine checks.
- Pass `--project` and location flags explicitly where applicable; use `--account` or `--configuration` to select existing settings without changing global defaults. Check fully qualified resource paths too: they can identify a different project. `--billing-project` selects the quota/billing project, not the resource target.
- Prefer named-resource `describe` calls or filtered lists. Select useful fields with `--format`; bound lists/logs with supported `--limit` and time filters. Limits do not bound individual payload size; report partial results.
- Use the execution runner's deadline for total runtime. A timeout or `--async` return does not mean a cloud operation stopped; inspect its operation/resource status before retrying. Verify readiness or the desired state, not just command acceptance.
- For writes, use `--quiet` only after explicit user confirmation of the displayed change set and resolution of all needed options. It accepts defaults and suppresses CLI prompts; it must not bypass the checkpoint.
- Deployment, deletion, restarts, IAM/network changes, API enablement, billing changes, and secret access must fit the user's request. Do not grant roles, create keys, enable APIs, or change credentials to bypass an access failure. Prefer the infrastructure source for managed resources unless a live override is requested.
- Judge side effects by the operation, not its name. Pub/Sub pulls affect message delivery even without acknowledgement; GKE `get-credentials` writes kubeconfig and changes its current context. Both require the write confirmation checkpoint.
- Use gcloud for service/resource management; use `kubectl`, `psql`, or `redis-cli` for workload or data queries. Report project, location, resource, evidence, and any access or coverage limits. Stop when the question is answered.

## Examples

Replace sample targets and names with resolved inputs. Run only the commands needed for the request; these examples use macOS zsh/bash.

```sh
# Inspect existing identities without printing credentials.
gcloud auth list --format='table(account,status)'

cloud_project='staging-project'
cloud_account='debug@example.com'
cloud_region='asia-southeast1'
cloud_cmd=(gcloud --project="$cloud_project" --account="$cloud_account")

# Inspect Cloud Run status without dumping environment variables.
"${cloud_cmd[@]}" run services describe api --region="$cloud_region" \
  --format='yaml(metadata.name,status.conditions,status.latestReadyRevisionName,status.traffic)'

# Read recent errors for one service; review/redact payloads before reporting.
"${cloud_cmd[@]}" logging read \
  'resource.type="cloud_run_revision" AND resource.labels.service_name="api" AND severity>=ERROR' \
  --freshness=1h --order=desc --limit=20 \
  --format='json(timestamp,severity,resource.labels,textPayload,jsonPayload.message)'

# Inspect GKE, Cloud SQL, and Memorystore control-plane status.
"${cloud_cmd[@]}" container clusters describe app --location="$cloud_region" \
  --format='yaml(name,location,status,currentMasterVersion)'
"${cloud_cmd[@]}" sql instances describe app-db \
  --format='yaml(name,region,state,databaseVersion,settings.tier)'
"${cloud_cmd[@]}" redis instances describe app-cache --region="$cloud_region" \
  --format='yaml(name,state,host,port,redisVersion,memorySizeGb)'

# Inspect a Compute Engine VM in its resolved zone.
"${cloud_cmd[@]}" compute instances describe worker --zone=asia-southeast1-a \
  --format='yaml(name,status,zone,machineType)'
```

Example request: “In staging-project, inspect Cloud Run service api in asia-southeast1 and explain its errors from the last hour. Do not redeploy or change IAM.”

Reference: [gcloud CLI reference](https://cloud.google.com/sdk/gcloud/reference/).
