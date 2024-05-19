# brew

An Ubuntu-based Docker image includes [Homebrew/brew].

[Homebrew/brew]: https://github.com/Homebrew/brew

# Links

- Source https://github.com/peaceiris/docker-images
- [peaceiris/brew - Docker Image](https://hub.docker.com/r/peaceiris/brew)
- [ghcr.io/peaceiris/brew - GitHub](https://github.com/peaceiris/docker-images/pkgs/container/brew)


# Usage


## docker run

```sh
docker run --rm -i -t --platform="linux/amd64" peaceiris/brew:latest bash
```

## GitHub Actions

```yaml
name: CI

on:
  push:

jobs:
  brew:
    runs-on: ubuntu-22.04
    container:
      image: "peaceiris/brew:latest"
    steps:
      - run: brew doctor
```
