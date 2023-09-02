# robotstxt

An Alpine Linux based Docker image includes a binary of google/robotstxt.

[google/robotstxt: The repository contains Google's robots.txt parser and matcher as a C++ library (compliant to C++11).](https://github.com/google/robotstxt)

```console
$ wget https://peaceiris.com/robots.txt
$ docker run --rm -v ./robots.txt:/root/robots.txt peaceiris/robotstxt:v0.1.0 /root/robots.txt Googlebot https://peaceiris.com/
user-agent 'Googlebot' with URI 'https://peaceiris.com/': ALLOWED
```

- [peaceiris/robotstxt - Docker Image](https://hub.docker.com/r/peaceiris/robotstxt)
- [ghcr.io/peaceiris/robotstxt - GitHub](https://github.com/peaceiris/docker-images/pkgs/container/robotstxt)
