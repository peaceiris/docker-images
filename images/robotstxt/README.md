# robotstxt

An Alpine Linux-based Docker image includes a binary of google/robotstxt.

[google/robotstxt: The repository contains Google's robots.txt parser and matcher as a C++ library (compliant to C++11).](https://github.com/google/robotstxt)


# Links

- Source https://github.com/peaceiris/docker-images
- [peaceiris/robotstxt - Docker Image](https://hub.docker.com/r/peaceiris/robotstxt)
- [ghcr.io/peaceiris/robotstxt - GitHub](https://github.com/peaceiris/docker-images/pkgs/container/robotstxt)


# Usage

## Docker

```console
$ wget https://peaceiris.com/robots.txt
$ docker run --rm -v ./robots.txt:/root/robots.txt peaceiris/robotstxt:v1.0.0 /root/robots.txt Googlebot https://peaceiris.com/
user-agent 'Googlebot' with URI 'https://peaceiris.com/': ALLOWED
```

## GitHub Actions

.github/workflows/robotstxt.yml

```yaml
name: robots.txt

on:
  workflow_dispatch:
  pull_request:
    branches:
      - main
    paths:
      - ".github/workflows/robotstxt.yml"
      - "assets/robots-*.txt"

jobs:
  validate:
    runs-on: ubuntu-22.04
    timeout-minutes: 3
    permissions:
      contents: read
    container:
      image: ghcr.io/peaceiris/robotstxt:v1.0.0
    steps:
      - uses: actions/checkout@v3
      - name: Validate
        run: |
          # Validate a robots.txt on a repository
          robots ./assets/robots.txt Googlebot https://peaceiris.com/
          robots ./assets/robots.txt GPTBot https://peaceiris.com/ || true

          # Download a robots.txt with the preinstalled curl and validate
          curl -s https://peaceiris.com/robots.txt --output robots.txt
          robots robots.txt Googlebot https://peaceiris.com/
          robots robots.txt GPTBot https://peaceiris.com/ || true
```
