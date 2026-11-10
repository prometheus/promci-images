# promci-images

GitHub Actions for publishing container images using Prometheus common Makefile
tooling.

## Usage

```yaml
jobs:
  publish:
    steps:
      ...
      - uses: prometheus/promci-artifacts/restore@<hash> # v0.1.0
      - uses: prometheus/promci-images/publish@<hash> # v0.1.0
        with:
          registry: docker.io
          organization: prome
          login: ${{ secrets.docker_hub_login }}
          password: ${{ secrets.docker_hub_password }}
  publish_release:
    permissions:
      contents: read
      id-token: write # keyless signing of release images
    steps:
      ...
      - uses: prometheus/promci-artifacts/restore@<hash> # v0.1.0
      - uses: prometheus/promci-images/publish_release@<hash> # v0.1.0
        with:
          registry: docker.io
          organization: prome
          login: ${{ secrets.docker_hub_login }}
          password: ${{ secrets.docker_hub_password }}
```

### Signed release images

`publish_release` signs every pushed release image by digest with
[cosign](https://github.com/sigstore/cosign) keyless signing, so the calling
job needs `id-token: write`. The certificate identity is the calling
repository's workflow at the release tag:

```sh
cosign verify quay.io/prometheus/node-exporter:v1.10.2 \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  --certificate-identity-regexp '^https://github\.com/prometheus/node_exporter/\.github/workflows/ci\.yml@refs/tags/v'
```

### Secondary images

For a subdirectory with its own Makefile (for example `generator/`), set
`working_directory` and skip multi-arch manifests when that Makefile has no
`docker-manifest` target:

```yaml
- uses: prometheus/promci-images/publish@<hash>
  with:
    registry: docker.io
    organization: prom
    login: ${{ secrets.docker_hub_login }}
    password: ${{ secrets.docker_hub_password }}
    working_directory: generator
    skip_manifest: true
```
