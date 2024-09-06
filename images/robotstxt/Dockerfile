FROM alpine:3.20.3 AS builder

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
RUN git checkout 455b1583103d13ad88fe526bc058d6b9f3309215
RUN cmake -S . -B c-build && \
    cmake --build c-build && \
    cmake --install c-build

FROM alpine:3.20.3
# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache \
    libstdc++ \
    curl
COPY --from=builder /usr/local/lib/librobots.so /usr/local/lib/librobots.so
COPY --from=builder /usr/local/bin/robots /usr/local/bin/robots
ENTRYPOINT ["robots"]
