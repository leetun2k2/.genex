# Install kubectl

Use this guide when the required CLI is missing or cannot be found on PATH.
The commands below target macOS. Reuse a working installation.

## Install

Requires [Homebrew](https://docs.brew.sh/Installation). If `brew` is unavailable,
follow its installation instructions and shell setup first.

```sh
brew install kubectl
```

## Verify and configure

```sh
kubectl version --client
kubectl config get-contexts
```

Use a client version compatible with the target cluster (within one minor version of the API server). Reuse kubeconfig; installation does not grant cluster access. Install a provider authentication plugin only when the existing kubeconfig requires it.

If a command is still missing, open a new terminal and check `command -v` and
PATH before reinstalling. Version checks verify the local binary, not live access.

Source: [installation documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/).
