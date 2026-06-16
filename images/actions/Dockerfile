FROM ubuntu:24.04

# https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL org.opencontainers.image.authors="peaceiris (Shohei Ueda)"
LABEL org.opencontainers.image.url="https://github.com/peaceiris/docker-images"
LABEL org.opencontainers.image.documentation="https://github.com/peaceiris/docker-images/blob/main/images/actions/README.md"
LABEL org.opencontainers.image.source="https://github.com/peaceiris/docker-images/blob/main/images/actions/Dockerfile"
LABEL org.opencontainers.image.description="An Ubuntu-based Docker image for GitHub Actions self-hosted runners."
LABEL org.opencontainers.image.licenses=MIT

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV DEBIAN_FRONTEND=noninteractive

# hadolint ignore=DL3008
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    curl \
    file \
    git \
    locales \
    procps \
    tzdata && \
    apt-get autoclean && \
    apt-get clean && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# Create runner user and switch to it
RUN useradd runner && \
    echo "runner:runner" | chpasswd && \
    chsh -s /usr/bin/bash runner && \
    usermod -aG sudo runner && \
    mkdir /actions-runner && \
    chown runner:runner /actions-runner
