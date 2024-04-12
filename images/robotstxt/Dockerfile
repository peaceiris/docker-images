FROM alpine:3.19.1 AS builder

WORKDIR /usr/local
# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache \
    ca-certificates \
    alpine-sdk \
    linux-headers \
    make \
    cmake \
    git
# hadolint ignore=DL3059
RUN git clone https://github.com/google/robotstxt.git
WORKDIR /usr/local/robotstxt
RUN git checkout 455b1583103d13ad88fe526bc058d6b9f3309215
WORKDIR /usr/local/robotstxt/c-build
RUN cmake .. -DROBOTS_BUILD_TESTS=ON
# hadolint ignore=DL3059
RUN make

FROM alpine:3.19.1
# hadolint ignore=DL3018
RUN apk add --update-cache --no-cache \
    libstdc++ \
    curl
COPY --from=builder /usr/local/robotstxt/c-build/librobots.so /usr/local/robotstxt/c-build/librobots.so
COPY --from=builder /usr/local/robotstxt/c-build/robots /usr/local/robotstxt/c-build/robots
ENV PATH="/usr/local/robotstxt/c-build:${PATH}"
ENTRYPOINT ["robots"]

# https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL org.opencontainers.image.authors="peaceiris (Shohei Ueda)"
LABEL org.opencontainers.image.url="https://github.com/peaceiris/docker-images"
LABEL org.opencontainers.image.documentation="https://github.com/peaceiris/docker-images/blob/main/images/robotstxt/README.md"
LABEL org.opencontainers.image.source="https://github.com/peaceiris/docker-images/blob/main/images/robotstxt/Dockerfile"
LABEL org.opencontainers.image.description="An Alpine Linux based Docker image includes a binary of google/robotstxt"
LABEL org.opencontainers.image.licenses=MIT
