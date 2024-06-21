# GitHub Actions self-hosted runner image

An Ubuntu-based Docker image for GitHub Actions self-hosted runners.


# Links

- Source https://github.com/peaceiris/docker-images
- [peaceiris/actions - Docker Image](https://hub.docker.com/r/peaceiris/actions)
- [ghcr.io/peaceiris/actions - GitHub](https://github.com/peaceiris/docker-images/pkgs/container/actions)


# Usage

## GitHub Actions

.github/workflows/ci.yml

```yaml
name: CI

on:
  workflow_dispatch:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-22.04
    timeout-minutes: 3
    permissions:
      contents: read
    container:
      image: ghcr.io/peaceiris/actions:v1.0.0
    steps:
      - uses: actions/checkout@v4
```
