# Harvester Updatecli POC

## Goal

POC for managing Harvester dependency and toolchain bumps with Updatecli.


### Go version update

- Automatically update the Go version in GitHub Actions, Dockerfile, and `go.mod`

### Go library updates in `go.mod`
In progress, expected to have
- Third-party Go libraries
- Kubernetes-related Go libraries

## Layout

`updatecli-compose/`

- Contains the Updatecli compose strategy used to automatically generate PRs

`updatecli/`

- Contains Updatecli manifests and shared configuration values

`updatecli/updatecli.d/`

- Contains Updatecli pipeline definitions

`updatecli/values/`

- Contains values that users can dynamically configure

## Local Run

Preview changes:

```bash
GITHUB_TOKEN=<token> updatecli compose diff -f updatecli-compose/golang-update.yaml
```

Apply changes:

```bash
GITHUB_TOKEN=<token> updatecli compose apply -f updatecli-compose/golang-update.yaml
```
