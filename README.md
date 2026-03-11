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
