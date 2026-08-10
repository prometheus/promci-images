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
