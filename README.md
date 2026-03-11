# Unleash Kubernetes Helm Chart

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Release](https://github.com/Unleash/helm-charts/actions/workflows/release.yaml/badge.svg?branch=main)](https://github.com/Unleash/helm-charts/actions/workflows/release.yaml)

## Instructions

[Helm](https://helm.sh) must be installed to use the charts. Please refer to Helm's [documentation](https://helm.sh/docs/) to get started.

### Add the Helm repository

```console
helm repo add unleash https://docs.getunleash.io/helm-charts
helm repo update
helm search repo unleash
```

### Environment structure

Environment-specific configurations live under `charts/unleash/`:

```
charts/unleash/
├── values.yaml                    # Environment-neutral defaults
├── values-testing.yaml            # Testing overrides (DB host, credentials, replicas, etc.)
├── values-prod.yaml               # Prod overrides (DB host, credentials, replicas, etc.)
├── virtualservice-testing.yaml    # Istio VirtualService for unleash.testing.sourceready.com
└── virtualservice-prod.yaml       # Istio VirtualService for unleash.sourceready.com
```

`values.yaml` contains environment-neutral defaults. Each `values-<env>.yaml` overrides only the values that differ per environment (database, SSL, replicas, etc.).

### Install Unleash

**Testing:**

```console
helm install unleash unleash/unleash \
  -n unleash --create-namespace \
  -f charts/unleash/values-testing.yaml

kubectl apply -f charts/unleash/virtualservice-testing.yaml
```

**Prod:**

```console
helm install unleash unleash/unleash \
  -n unleash --create-namespace \
  -f charts/unleash/values-prod.yaml

kubectl apply -f charts/unleash/virtualservice-prod.yaml
```

Dry-run to preview rendered manifests without actually installing:

```console
helm install unleash unleash/unleash \
  -n unleash --create-namespace \
  -f charts/unleash/values-testing.yaml \
  --debug --dry-run
```

### Upgrade Unleash

**Testing:**

```console
helm upgrade unleash unleash/unleash \
  -n unleash \
  -f charts/unleash/values-testing.yaml
```

**Prod:**

```console
helm upgrade unleash unleash/unleash \
  -n unleash \
  -f charts/unleash/values-prod.yaml
```

### Uninstall Unleash

```console
helm uninstall unleash -n unleash
```

### Render templates locally

```console
helm template unleash unleash/unleash \
  -n unleash \
  -f charts/unleash/values-testing.yaml
```

### Run Super-Linter locally (requires Docker)

```bash
docker run --rm \
    -e RUN_LOCAL=true \
    --env-file ".github/super-linter.env" \
    -v "$(pwd)":/tmp/lint \
    ghcr.io/super-linter/super-linter:latest
```

## Reference links

| Resource | URL |
|----------|-----|
| Helm | https://helm.sh |
| Helm documentation | https://helm.sh/docs/ |
| Kubernetes support period | https://kubernetes.io/releases/patch-releases/#support-period |
| Unleash | https://unleash.github.io/ |
| This repository | https://github.com/unleash/helm-charts/ |
| Contribution guidelines | https://github.com/unleash/helm-charts/blob/main/CONTRIBUTING.md |
| License | https://github.com/unleash/helm-charts/blob/main/LICENSE |
| Unleash Helm charts (docs) | https://docs.getunleash.io/helm-charts |
| Release workflow | [.github/workflows/release.yaml](https://github.com/unleash/helm-charts/blob/main/.github/workflows/release.yaml) |

<!-- Keep full URL links to repo files because this README syncs from main to gh-pages. -->

## Kubernetes support strategy

We'll build this repository on all k8s versions that have not reached End of Life according to the [Kubernetes support period](https://kubernetes.io/releases/patch-releases/#support-period).

## Contributing

The source code of all [unleash](https://unleash.github.io/) [Helm](https://helm.sh) charts can be found on Github: <https://github.com/unleash/helm-charts/>

We'd love to have you contribute! Please refer to our [contribution guidelines](https://github.com/unleash/helm-charts/blob/main/CONTRIBUTING.md) for details.

## License

[Apache 2.0 License](https://github.com/unleash/helm-charts/blob/main/LICENSE).

## Helm charts build status

[![Release](https://github.com/Unleash/helm-charts/actions/workflows/release.yaml/badge.svg?branch=main)](https://github.com/Unleash/helm-charts/actions/workflows/release.yaml)

## Releasing the charts

To release new versions of the charts, you must update the chart version in the Chart.yaml file and then merge these modifications into the main branch. Following this, the workflow will detect these changes and automatically release a new version of the modified Helm chart.

## Helm repository

This repository employs a process that converts it into a Helm repository, specifically using the Helm Chart Releaser Action version 1.5.0. It leverages GitHub Pages for hosting the artifacts. Furthermore, the [Unleash documentation on Helm charts](https://docs.getunleash.io/helm-charts) utilizes a CNAME to direct to the GitHub Pages.

The specific workflow is outlined in the file located at .github/workflows/release.yml. This workflow activates whenever there's an update to the chart version, which consequently prompts an update to the repository with the new chart version.
