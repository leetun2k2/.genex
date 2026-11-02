---
name: 0-kubectl
description: Use kubectl to inspect Kubernetes workloads, logs, events, networking, and resource usage, debug failures, and carry out requested cluster changes.
---

# Kubernetes

Use the installed `kubectl`. Choose commands based on the user's goal; use `kubectl <command> --help` and `kubectl explain` when needed.

## Read and write policy

- Run in-scope read-only operations without user confirmation. Keep filters, limits, and timeouts as operational safeguards.
- Before any write, mutation, or configuration change, including local edits, prepare a concrete change set. Immediately before execution, show the target, exact commands with secrets redacted, and a diff or effects summary when no text diff is available. Ask for explicit user confirmation and wait for it. The initial task request alone does not satisfy this checkpoint.
- Confirmation covers the displayed change set, including repetitive sub-steps within it. Ask again only if the target, proposed changes, or side effects change.

## Inputs

Resolve these from the request, project config, or existing kubeconfig. Ask only when a missing fact prevents useful work.

| Input | Details |
| --- | --- |
| Target | Environment, kubeconfig/context, and namespace. Verify the intended cluster; the current context alone does not establish intent. |
| Goal | What to inspect, debug, or change; resource kind/name, labels, container, and time window when relevant. |
| Change scope | For changes: allowed resources, operation or manifest, desired state, and recovery plan where relevant. Investigation is read-only by default. |

## Usage

- Reuse existing kubeconfig and authentication. Pass `--context` and `--namespace` explicitly; avoid changing the user's current context. Keep credentials, Secret values, and sensitive logs out of output and files; do not dump raw kubeconfig or bypass TLS verification.
- Scope reads to named resources or label selectors. Inspect status, events, and relevant logs before choosing deeper checks. Use `auth can-i` for permission failures; do not elevate permissions automatically.
- Bound logs with `--since`, `--tail`, and `--limit-bytes`. Select the container in multi-container pods; use `--previous` for the last terminated container instance. Avoid unbounded watches or log streams.
- Use `--request-timeout` for individual API requests and the execution runner's deadline for total runtime. Set `--timeout` on waits and rollout checks. Metrics may be unavailable; `top` shows current usage, not historical peaks.
- Judge `exec` by the command it runs: it can mutate data or call external services. `debug` can add containers or create pods. Use them only within the requested scope. For port forwarding, bind to loopback and stop the forwarding process when done.
- Include apply/patch, deletion, restarts, scaling, node maintenance, mutating `exec`/`debug` actions, and access/Secret or kubeconfig changes in the write confirmation checkpoint. Do not use broad deletion, force deletion, or conflict overrides to bypass a failure.
- Before applying a manifest, inspect its namespaces and cluster-scoped resources and preview with `diff` or server dry-run as appropriate. `diff` exits `0` for no changes, `1` for differences, and greater than `1` for errors. For GitOps-managed resources, update the source unless a live override is requested.
- Verify requested changes through rollout/readiness and resource state; API acceptance alone does not prove success. Report context, namespace, resources, evidence, and any access or coverage limits. Stop when the question is answered.

## Examples

Replace sample targets, labels, and names with resolved inputs. Run only the commands needed for the request; these examples use macOS zsh/bash.

```sh
# Discover local contexts without changing the active context.
kubectl config get-contexts

kube_context='staging'
kube_namespace='app'
kube_pod='api-abc123'
kube_cmd=(kubectl --context "$kube_context" --namespace "$kube_namespace" --request-timeout=10s)

# Inspect workload state and a failing pod.
"${kube_cmd[@]}" get deployments,pods -l app=api -o wide
"${kube_cmd[@]}" describe pod "$kube_pod"
"${kube_cmd[@]}" get events --field-selector "involvedObject.kind=Pod,involvedObject.name=$kube_pod" --sort-by=.lastTimestamp

# Read bounded logs; --previous applies only if a prior instance exists.
"${kube_cmd[@]}" logs "$kube_pod" -c api --since=15m --tail=100 --limit-bytes=16384 --timestamps
"${kube_cmd[@]}" logs "$kube_pod" -c api --previous --tail=100 --limit-bytes=16384

# Inspect service routing and current resource usage.
"${kube_cmd[@]}" get service api -o wide
"${kube_cmd[@]}" get endpointslices -l kubernetes.io/service-name=api
"${kube_cmd[@]}" top pods -l app=api --containers
"${kube_cmd[@]}" auth can-i get pods

# Preview a proposed manifest; differences return exit code 1.
"${kube_cmd[@]}" diff -f /path/to/deployment.yaml
```

For a requested deployment update, after showing the target, apply command, and manifest diff and receiving explicit user confirmation:

```sh
"${kube_cmd[@]}" apply -f /path/to/deployment.yaml
"${kube_cmd[@]}" rollout status deployment/api --timeout=60s
```

Example request: “In staging context, namespace app, find why the api pods keep restarting. Inspect events and previous logs; report the cause without restarting anything.”

Reference: [kubectl documentation](https://kubernetes.io/docs/reference/kubectl/).
