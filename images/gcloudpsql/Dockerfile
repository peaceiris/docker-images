ARG GCLOUD_SDK_VERSION
FROM google/cloud-sdk:${GCLOUD_SDK_VERSION}-alpine

# https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL org.opencontainers.image.authors="peaceiris (Shohei Ueda)"
LABEL org.opencontainers.image.url="https://github.com/peaceiris/docker-images"
LABEL org.opencontainers.image.documentation="https://github.com/peaceiris/docker-images/blob/main/images/gcloudpsql/README.md"
LABEL org.opencontainers.image.source="https://github.com/peaceiris/docker-images/blob/main/images/gcloudpsql/Dockerfile"
LABEL org.opencontainers.image.description="An Alpine Linux-based Docker image includes gcloud and psql"
LABEL org.opencontainers.image.licenses=MIT

SHELL ["/bin/bash", "-eo", "pipefail", "-c"]

ARG POSTGRESQL_VERSION
# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache "postgresql${POSTGRESQL_VERSION}-client" coreutils && \
    psql --version && \
    gcloud --version
