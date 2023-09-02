DOCKER_CLI_EXPERIMENTAL := enabled
DOCKER_BUILDKIT := 1

DOCKER_USERNAME ?= peaceiris
DOCKER_IMAGE_NAME := robotstxt
DOCKER_HUB_BASE_NAME := ${DOCKER_USERNAME}/${DOCKER_IMAGE_NAME}
DOCKER_BASE_NAME := ghcr.io/${DOCKER_HUB_BASE_NAME}
DOCKER_VERSION := 0.1.0
DOCKER_TAG := v${DOCKER_VERSION}
DOCKER_PLATFORM ?= linux/arm64
GITHUB_REF_NAME ?= local
DOCKER_SCOPE := robotstxt-${GITHUB_REF_NAME}
PKG_NAME := ${DOCKER_BASE_NAME}:${DOCKER_TAG}
HUB_NAME := ${DOCKER_HUB_BASE_NAME}:${DOCKER_TAG}

.PHONY: login-dockerhub
login-dockerhub:
	echo "${DOCKER_HUB_TOKEN}" | docker login -u "${DOCKER_USERNAME}" --password-stdin

.PHONY: login-ghcr
login-ghcr:
	echo "${GITHUB_TOKEN}" | docker login ghcr.io -u "${DOCKER_USERNAME}" --password-stdin

.PHONY: login
login: login-dockerhub login-ghcr

.PHONY: setup-buildx
setup-buildx:
	docker buildx create --use --driver docker-container
	docker version

.PHONY: build
build: setup-buildx
	docker buildx build . \
		--tag "${PKG_NAME}" \
		--platform "${DOCKER_PLATFORM}" \
		--label "org.opencontainers.image.version=${DOCKER_VERSION}" \
		--output "type=docker" \
		--cache-from "type=gha,scope=${DOCKER_SCOPE}" \
		--cache-to "type=gha,mode=max,scope=${DOCKER_SCOPE}"
	docker image inspect "${PKG_NAME}" --format='{{json .Config.Labels}}' | jq
	docker tag "${PKG_NAME}" "${HUB_NAME}"
	docker images

.PHONY: push
push:
	docker push "${PKG_NAME}"
	docker push "${HUB_NAME}"
