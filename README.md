# helm-workflows

Reusable GitHub Actions workflows for linting and releasing Helm charts.

The release workflow creates releases with Release Please, injects the required dynamic OCI annotations, publishes the chart to its owner-scoped Zot repository, and signs and verifies the immutable digest with Notation.

## Signed release workflow

```yaml
name: Release Helm Chart

on:
  push:
    branches: [main]

permissions:
  contents: write
  pull-requests: write

jobs:
  release:
    uses: Jmainguy/helm-workflows/.github/workflows/helm-release.yml@v1
    with:
      chart_name: helm-webapp
      chart_path: .
      helm_registry: zot.soh.re
      registry_namespace: jmainguy
    secrets:
      release_token: ${{ secrets.RELEASE_PLEASE_TOKEN }}
      helm_username: ${{ secrets.HELM_USERNAME }}
      helm_password: ${{ secrets.HELM_PASSWORD }}
      notation_private_key: ${{ secrets.NOTATION_PRIVATE_KEY }}
      notation_certificate_chain: ${{ secrets.NOTATION_CERTIFICATE_CHAIN }}
      notation_ca_certificate: ${{ secrets.NOTATION_CA_CERTIFICATE }}
```

The chart must provide the stable required OCI annotations in `Chart.yaml`. The workflow supplies `created`, `revision`, and `version` at release time.
