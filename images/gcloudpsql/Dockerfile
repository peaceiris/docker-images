ARG GCLOUD_SDK_VERSION
FROM google/cloud-sdk:${GCLOUD_SDK_VERSION}-alpine

SHELL ["/bin/bash", "-eo", "pipefail", "-c"]

ARG POSTGRESQL_VERSION
# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache "postgresql${POSTGRESQL_VERSION}-client" coreutils && \
    psql --version && \
    gcloud --version
