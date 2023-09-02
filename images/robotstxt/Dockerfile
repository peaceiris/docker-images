FROM debian:12-slim AS builder

WORKDIR /usr/local
# hadolint ignore=DL3008
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    build-essential \
    git \
    cmake
RUN git clone https://github.com/google/robotstxt.git
WORKDIR /usr/local/robotstxt
RUN git checkout 455b1583103d13ad88fe526bc058d6b9f3309215
WORKDIR /usr/local/robotstxt/c-build
RUN cmake .. -DROBOTS_BUILD_TESTS=ON
# hadolint ignore=DL3059
RUN make

FROM debian:12-slim
COPY --from=builder /usr/local/robotstxt/c-build /usr/local/robotstxt/c-build
ENV PATH "${PATH}:/usr/local/robotstxt/c-build"
ENTRYPOINT ["/usr/local/robotstxt/c-build/robots"]

# https://github.com/opencontainers/image-spec/blob/main/annotations.md
LABEL org.opencontainers.image.authors="peaceiris (Shohei Ueda)"
LABEL org.opencontainers.image.url="https://github.com/peaceiris/docker-images"
LABEL org.opencontainers.image.documentation="https://github.com/peaceiris/docker-images/blob/main/images/robotstxt/README.md"
LABEL org.opencontainers.image.source="https://github.com/peaceiris/docker-images/blob/main/images/robotstxt/Dockerfile"
LABEL org.opencontainers.image.description="A Docker image includes a binary of google/robotstxt"
LABEL org.opencontainers.image.licenses=MIT
