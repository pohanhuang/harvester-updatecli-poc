# Golang Version Policy

This policy bundles three Updatecli pipelines for bumping the Go version used by
Harvester-related repositories.

It can update:

- `Dockerfile.dapper`
- `go.mod`
- GitHub Actions workflows that use `actions/setup-go`

## Files

- `updatecli.d/docker.yaml`: updates the Go image tag in `Dockerfile.dapper`
- `updatecli.d/gomod.yaml`: updates the Go version declared in `go.mod`
- `updatecli.d/gha.yaml`: updates `go-version` in GitHub Actions workflows
- `updatecli.d/_scm.github.yaml`: shared GitHub SCM and pull request configuration
- `values.yaml`: default customer-facing values

## Required Values

At minimum, set these values before using the policy:

```yaml
scm:
  enabled: true
  kind: github
  env_token: GITHUB_TOKEN
  owner: suse
  repository: harvester
  branch: master
  username: harvester-bot
  user: harvester-bot
  email: harvester-bot@users.noreply.github.com
```

Optional pull request settings:

```yaml
actions:
  reviewers:
    - harvester/dev
  labels:
    - golang
    - dependency
```

## Local Usage

Render the policy locally:

```sh
updatecli manifest show --config updatecli.d --values values.yaml
```

Preview changes:

```sh
updatecli diff --config updatecli.d --values values.yaml
```

Apply changes:

```sh
updatecli apply --config updatecli.d --values values.yaml
```

## OCI Usage

Publish the policy bundle:

```sh
updatecli manifest push \
  --config updatecli.d \
  --values values.yaml \
  --policy Policy.yaml \
  --tag ghcr.io/pohanhuang/harvester/mock \
  .
```

Consume the published policy:

```sh
updatecli manifest show ghcr.io/pohanhuang/harvester/mock:0.1.0 --values /absolute/path/to/mock.yaml
```

When using an OCI policy, prefer an absolute path for `--values`.

## Notes

- `gha.yaml` only updates workflows that actually use `actions/setup-go`
- `manifest show` can render token values, so avoid sharing its raw output
- Any extra `--values` file overrides defaults from `values.yaml`
