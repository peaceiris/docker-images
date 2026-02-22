FROM alpine:3.21.3@sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c AS builder

# https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL org.opencontainers.image.authors="peaceiris (Shohei Ueda)"
LABEL org.opencontainers.image.url="https://github.com/peaceiris/docker-images"
LABEL org.opencontainers.image.documentation="https://github.com/peaceiris/docker-images/blob/main/images/robotstxt/README.md"
LABEL org.opencontainers.image.source="https://github.com/peaceiris/docker-images/blob/main/images/robotstxt/Dockerfile"
LABEL org.opencontainers.image.description="An Alpine Linux based Docker image includes a binary of google/robotstxt"
LABEL org.opencontainers.image.licenses=MIT

# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache \
    ca-certificates \
    alpine-sdk \
    linux-headers \
    make \
    cmake \
    git
WORKDIR /usr/local
# hadolint ignore=DL3059
RUN git clone https://github.com/google/robotstxt.git
WORKDIR /usr/local/robotstxt
# v1.0.0
RUN git checkout a7a82530abedfda75f6a81396666700fc11e89fd
RUN cmake -S . -B c-build && \
    cmake --build c-build && \
    cmake --install c-build

FROM alpine:3.21.3@sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c
# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache \
    libstdc++ \
    curl
COPY --from=builder /usr/local/lib/librobots.so /usr/local/lib/librobots.so
COPY --from=builder /usr/local/bin/robots /usr/local/bin/robots
ENTRYPOINT ["robots"]
